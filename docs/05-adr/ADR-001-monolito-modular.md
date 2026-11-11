# ADR-001: Adoção de monolito modular

Esta seção registra a decisão de como o back-end do Neighborhood Action Dashboard é dividido e implantado. Ela sustenta a [abordagem arquitetural](../02-arquitetura-da-solucao.md) e as [visões 4+1](../03-visoes-4mais1/03.2-visao-desenvolvimento.md), e segue o formato de Nygard (2011).

- **Status:** Aceita
- **Data:** 11/11/2026
- **Decisores:** José Ribeiro dos Santos Neto; Danilo Roberto Anterio dos Santos

## Contexto

O sistema reúne cinco capacidades de negócio (`ocorrencias`, `priorizacao`, `notificacoes`, `identidade` e `administracao`) e quatro sistemas externos (gov.br, serviço de mapas e geocodificação, serviço de e-mail e push, armazenamento de objetos). A decisão sobre como dividir e implantar essas capacidades sofre as seguintes forças, que puxam em direções diferentes:

| Força | Descrição | Requisito |
|---|---|---|
| Equipe e prazo | Equipe de 2 integrantes e prazo curto. Tudo o que se implanta, monitora e depura tem de caber nessa capacidade. | – |
| Desempenho | Consulta e registro com p95 ≤ 200 ms sob 100 req/s, sem contar o upload de fotos. O registro de uma ocorrência consulta categoria, localização e pontuação no mesmo caso de uso. | RNF01 |
| Disponibilidade | 99,5% ao mês, backup diário, RPO ≤ 24 h e RTO ≤ 4 h. Quanto menos peças em operação, menos pontos de falha e mais simples a recuperação. | RNF04 |
| Manutenibilidade | Regras de negócio independentes de framework, banco e interface, com cobertura ≥ 80% em domínio e aplicação. As fronteiras entre módulos precisam ser explícitas e verificáveis. | RNF06 |
| Consistência | O registro, a mudança de status e o evento de notificação precisam ser gravados juntos. A falha de envio não pode desfazer a mudança de status. | RF01, RF03, RF06 |
| Evolução | Os módulos têm graus de maturidade diferentes (D6) e `notificacoes` é o mais candidato a uma futura extração. | RNF06 |

A questão é como obter fronteiras claras entre módulos sem pagar o custo operacional de um sistema distribuído.

## Decisão

Decidimos adotar um **monólito modular**: um único back-end implantável (Java LTS com Spring Boot) organizado nos cinco módulos citados, cada um como pacote de topo com seus círculos internos da Clean Architecture (MARTIN, 2019), conforme a [Visão de Desenvolvimento](../03-visoes-4mais1/03.2-visao-desenvolvimento.md).

Regras que tornam a decisão verificável:

- Os módulos só conversam pela API pública do módulo ou por eventos de domínio. O Spring Modulith verifica essa regra em teste, e a violação quebra a construção.
- A comunicação entre módulos é uma chamada em memória, dentro da transação do caso de uso.
- A notificação usa evento de domínio com registro de publicação (outbox) gravado na mesma transação, e um listener assíncrono envia depois. Não há broker de mensagens.
- O banco PostgreSQL com PostGIS é único, mas cada módulo é dono das suas tabelas e não lê tabelas de outro módulo (D7).
- A API é stateless (JWT), então pode rodar em várias instâncias da mesma aplicação.

## Alternativas consideradas

Todas as alternativas foram avaliadas pelos mesmos critérios, derivados das forças do contexto.

| Critério | Monólito modular (escolhido) | Monólito sem módulos | Microsserviços | Serverless (funções por operação) |
|---|---|---|---|---|
| Cabe em equipe de 2 e prazo curto | Sim | Sim | Não | Parcial |
| Latência do registro (RNF01) | Chamadas em memória | Chamadas em memória | Chamadas de rede entre serviços | Partida a frio e conexões ao banco por função |
| Peças a operar e recuperar (RNF04) | 1 aplicação, 1 banco | 1 aplicação, 1 banco | Vários serviços e vários bancos | Muitas funções e serviços gerenciados |
| Fronteiras verificáveis (RNF06) | Sim, pelo Spring Modulith | Não | Sim, pela rede | Parcial, por função |
| Transação única no registro (RF01, RF06) | Sim | Sim | Não, exige consistência eventual | Não por padrão |

### Motivo do descarte

| Alternativa | Por que foi descartada |
|---|---|
| Monólito sem módulos | É o mais simples no início, mas não impõe fronteira alguma. Sem fronteiras, o acoplamento cresce e o RNF06 depende só da disciplina da equipe, sem verificação automática. Também fecha a porta para extrair um módulo no futuro. |
| Microsserviços | A escalabilidade independente por serviço não é necessária para a carga de 100 req/s do RNF01. Em troca, exige comunicação por rede, um banco por serviço, tratamento de consistência eventual no registro e na priorização, observabilidade distribuída e implantação coordenada. Para 2 integrantes e prazo curto, esse custo operacional supera o benefício e ameaça o RNF04. |
| Serverless | Elimina a gestão de servidores, mas espalha o caso de uso em várias funções. Complica a transação única entre ocorrência e evento (RF06), pode somar partidas a frio ao p95 do RNF01 e dificulta o teste local e a tarefa agendada de recálculo de pontuação. Também prende o projeto a recursos específicos do provedor. |

## Consequências

**Positivas:**

- **Desempenho (RNF01):** o registro e a consulta não pagam latência de rede entre módulos. As chamadas ficam em memória e a transação é única.
- **Consistência (RF01, RF03, RF06):** a mudança de status, o `HistoricoStatus` e o evento de notificação são gravados juntos, sem coordenação distribuída.
- **Disponibilidade (RNF04):** há uma aplicação e um banco para monitorar, copiar e restaurar. Como a API não guarda sessão, várias instâncias reduzem o impacto da falha de uma.
- **Manutenibilidade (RNF06):** o Spring Modulith transforma as fronteiras em regra testável. O domínio permanece Java puro e testável sem Spring e sem banco.
- **Prazo:** um único repositório, uma única implantação e um único conjunto de testes de integração com o banco.
- **Evolução:** um módulo com fronteira verificada pode ser extraído depois, em especial `notificacoes`, que já é reativo a eventos.

**Negativas:**

- **Disciplina:** as fronteiras só valem enquanto os testes do Spring Modulith estiverem na construção. Sem isso, o monólito modular degrada para monólito comum.
- **Escala e falha em bloco:** a escala é da aplicação inteira, mesmo que só um módulo precise de mais capacidade. Uma falha de implantação ou um defeito grave afeta todos os módulos de uma vez.
- **Banco compartilhado:** a separação por módulo no banco é uma convenção. Uma consulta que cruza tabelas de módulos diferentes é tecnicamente possível e precisa ser barrada em revisão de código.
- **Ciclo de implantação único:** uma mudança pequena em `administracao` obriga a reimplantar o sistema inteiro.
- **Custo da Clean:** a combinação com a Clean Architecture mantém mais interfaces e mapeadores domínio ↔ JPA no módulo `ocorrencias`, como já registrado na [seção 2](../02-arquitetura-da-solucao.md).
- **Tecnologia única:** todo o back-end usa a mesma plataforma (Java e Spring). Um módulo não pode adotar outra linguagem sem ser extraído.
- **Tarefa agendada:** com várias instâncias, o recálculo de pontuação por tempo em aberto precisa rodar em uma instância por vez, o que exige um mecanismo de exclusão.

## Critérios de revisão

Reavaliamos esta decisão se ocorrer qualquer um dos casos abaixo:

| Gatilho | Sinal observável | Reação provável |
|---|---|---|
| Carga acima do dimensionado | p95 acima de 200 ms sob 100 req/s (RNF01) que persista após ajuste de índices, paginação e pool de conexões | Estudar a extração do módulo responsável pela carga |
| Disponibilidade | Meta de 99,5% mensal (RNF04) descumprida por acoplamento entre módulos | Isolar o módulo que causa as falhas |
| Fronteiras violadas | Falhas recorrentes nos testes do Spring Modulith ou dependências cíclicas entre módulos (RNF06) | Revisar o desenho dos módulos antes de qualquer extração |
| Crescimento da equipe | Desenvolvimento dividido entre equipes diferentes, cada uma dona de módulos distintos e com ciclos de entrega independentes | Avaliar equipes por módulo e implantação separada |
| Requisito de escala ou tecnologia por módulo | Um módulo precisa de escala ou de plataforma diferente dos demais | Extrair esse módulo como serviço próprio, preservando a API pública e os eventos |

A revisão é registrada em um novo ADR que substitui este, sem apagar o histórico.

**Requisitos atendidos:** RF01, RF03, RF06, RNF01, RNF04, RNF06
