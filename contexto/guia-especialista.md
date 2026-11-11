# Guia do Especialista – critérios de qualidade por artefato

Use este guia para escrever E para revisar. Se uma seção não passa na autorrevisão (seção 7), não está pronta.

## 1. Clean Architecture aplicada ao domínio

| Círculo | Conteúdo neste projeto |
|---|---|
| Entidades | Ocorrencia (regra de transição de status, invariantes), Categoria (peso de risco), Apoio; serviço de domínio PoliticaDePriorizacao |
| Casos de uso | RegistrarOcorrencia, AcompanharOcorrencia, AtualizarSituacao, PriorizarDemanda, EncerrarSolicitacao, ApoiarOcorrencia, ConsultarPainel |
| Adaptadores de interface | Controllers REST, DTOs/presenters, implementações de repositório (JPA) e gateways |
| Frameworks e drivers | Spring Boot (Spring Web, Spring Security, Spring Data JPA, Spring Modulith), PostgreSQL/PostGIS, armazenamento de objetos, provedor gov.br, serviço de e-mail/push |

- Regra de dependência: dependências de código-fonte apontam sempre para dentro.
- Portas (interfaces) declaradas no círculo de casos de uso: OcorrenciaRepository, GeocodificacaoGateway,
  ArmazenamentoDeArquivos, Notificador, Relogio. Implementações ficam nos adaptadores (inversão de dependência – DIP).
- Fronteiras são atravessadas por DTOs; entidades não chegam ao controller nem ao banco diretamente.
- Onde fica cada validação: formato/obrigatoriedade → DTO no adaptador (Bean Validation); regra de negócio → entidade/caso de uso.
- Java/Spring: entidades de domínio e casos de uso são classes Java puras (sem @Entity, @Service, @Autowired).
  Entidades JPA ficam no adaptador de persistência, com mapper domínio ↔ JPA. A injeção de dependência do
  Spring só monta as peças na borda (configuração), nunca dentro do domínio.
- Monólito modular: cada módulo (ocorrencias, priorizacao, notificacoes, identidade, administracao) é um
  pacote de topo com seus círculos internos; o Spring Modulith verifica que módulos só conversam pela API
  pública ou por eventos.

Erros que tiram nota: confundir Clean com Camadas; caso de uso chamando JPA/ORM; regra de negócio no controller;
entidade anêmica; anotações de framework no domínio; não explicar o custo extra (mais interfaces, mappers,
curva de aprendizado).

## 2. Modelo 4+1 (Kruchten, 1995)

| Visão | Público | Preocupação | Notação recomendada | Conteúdo mínimo aqui |
|---|---|---|---|---|
| Lógica | Usuário final, analista | Funcionalidade | Diagrama de classes + diagrama de estados | Entidades canônicas, atributos-chave, multiplicidades, ciclo de status |
| Desenvolvimento | Desenvolvedores | Organização do código | Diagrama de pacotes/componentes | Módulos do monólito × círculos da Clean, setas de dependência para o domínio, front React separado |
| Processo | Integradores, operação | Concorrência, distribuição, desempenho, disponibilidade | Diagrama de atividades/fluxo com raias | Síncrono (HTTPS/REST) × assíncrono (outbox + listener), sistemas externos, pontos de contenção |
| Física | Infraestrutura | Implantação | (não exigida – citar em 1 linha) | – |
| Cenários (+1) | Todos | Validação e integração das visões | Caso de uso + tabela visão × contribuição | Registrar ocorrência |

Concorrência que a Visão de Processo deve tratar:
- Apoios simultâneos → restrição de unicidade (cidadao_id, ocorrencia_id) em Apoio.
- Dois gestores alterando o mesmo status → versionamento otimista (@Version em Ocorrencia), conflito devolvido ao usuário.
- Notificação → evento registrado na mesma transação (registro de publicação do Spring Modulith = outbox);
  listener assíncrono envia e faz nova tentativa em caso de falha (RF06).
- Recalcular prioridade → no momento da escrita (apoio/mudança) mais atualização agendada pelo tempo em aberto.
- Desempenho (RNF01) → índices espaciais (PostGIS), paginação, upload de foto fora do caminho crítico,
  pool de conexões dimensionado.

Erros que tiram nota: repetir o mesmo diagrama em visões diferentes; a Visão de Processo ser uma cópia da sequência
da Etapa 4; não citar concorrência; misturar notação ou termos do C4.

## 3. Diagrama de sequência
- Participantes com os MESMOS nomes da Visão de Desenvolvimento.
- Mensagem síncrona `->>`, retorno `-->>`; `autonumber`; `alt`/`else`, `opt`, `Note`.
- Todo fluxo alternativo tem condição de guarda [ ] e um desfecho claro para o usuário.
- A tabela # | De | Para | Mensagem e o diagrama têm a mesma numeração.
- Respeitar a arquitetura: controller → caso de uso → entidade/porta → adaptador → banco. Nunca controller → banco.

## 4. ADR (formato de Nygard)
- Uma decisão por ADR, na voz ativa ("Decidimos adotar…").
- O contexto descreve forças em conflito (prazo, equipe, RNFs), não a solução.
- Alternativas avaliadas com os mesmos critérios e descarte justificado.
- As consequências negativas são reais e específicas (nada de "nenhuma").
- Inclui critério de revisão: quando a decisão deve ser reavaliada.

## 5. Testes
- Pirâmide com proporção indicativa (~70% unitários, ~20% integração, ~10% E2E) e justificativa.
- Unitários testam o domínio puro (sem Spring, sem banco); isso só é possível por causa da Clean (RNF06).
  Ferramentas de referência: JUnit 5 e Mockito.
- Integração: API + banco PostgreSQL real em ambiente isolado (ex.: Testcontainers); testes de fronteira de
  módulos com Spring Modulith.
- E2E pelo fluxo do usuário no front (ex.: Playwright ou Cypress).
- Cada exemplo no formato Dado / Quando / Então, com RF citado.
- Na DoD, cada critério tem uma evidência verificável.

## 6. Padrão de justificativa (use em toda decisão)
**Decisão** → **Motivação** (RF/RNF ligado) → **Benefícios** → **Limitações/custos** → **Alternativas descartadas (por quê)**.

## 7. Autorrevisão antes de entregar qualquer seção
- [ ] Atende 100% dos itens da etapa no enunciado-resumo.
- [ ] Cita RF/RNF por ID e todos existem no docs/01.
- [ ] Nomes de entidades, status, papéis, módulos e sistemas externos iguais ao modelo canônico.
- [ ] Stack citada é a da D5 (Java/Spring), sem outra tecnologia de back-end.
- [ ] Toda decisão segue o padrão da seção 6.
- [ ] Mermaid válido segundo mermaid.instructions.md.
- [ ] Nada de C4, código de implementação ou ferramenta de infraestrutura fora de ADR.
- [ ] Nenhum número ou nome inventado; pendências como [PLACEHOLDER].
- [ ] Citações no formato autor-data (ex.: KRUCHTEN, 1995) presentes em docs/07.
