# 1. Documento de Visão

Esta seção apresenta o problema de negócio, os benefícios esperados, os papéis dos usuários e os requisitos que orientam as decisões arquiteturais das seções seguintes.

## 1.1 Objetivo, problema, público-alvo e benefícios esperados

| Elemento | Descrição |
|---|---|
| **Objetivo da solução** | O Neighborhood Action Dashboard é uma plataforma digital única para registrar, organizar, priorizar e acompanhar demandas urbanas de bairros e comunidades. Exemplos: iluminação pública, acessibilidade, praças e parques, resíduos, buracos em vias, sinalização e equipamentos públicos. Ela liga o cidadão que identifica o problema ao gestor público que o resolve, com transparência em todas as etapas. |
| **Problema a ser resolvido** | As demandas chegam por canais diferentes (telefone, e-mail, redes sociais, atendimento presencial, associações de bairro), sem padrão de informação e sem um identificador único. Isso gera quatro problemas. (1) Registros incompletos, sem localização precisa nem evidência. (2) Duplicidade: o mesmo problema é relatado várias vezes e tratado como demandas diferentes. (3) A priorização é feita por ordem de chegada ou por pressão, sem critério objetivo. (4) O cidadão não sabe em que situação está a solicitação e o gestor não tem visão consolidada do território. |
| **Público-alvo** | **Usuários diretos:** cidadãos e moradores (registram e acompanham), gestores públicos de secretarias municipais (por exemplo, obras, iluminação e limpeza urbana), que analisam, priorizam e resolvem, e administradores da plataforma (por exemplo, a equipe de TI da prefeitura). **Beneficiados indiretos:** associações de moradores, lideranças comunitárias e a comunidade em geral. |
| **Benefícios esperados** | Um único ponto de entrada padronizado, com protocolo rastreável. Menos registros duplicados e incompletos. Priorização baseada em critérios objetivos e em dados. Menor tempo entre o registro e a resolução. Transparência sobre o andamento. Base histórica para planejar a manutenção urbana por bairro e por categoria. |

## 1.2 Benefícios da solução

| Para cidadãos | Para gestores públicos | Para a comunidade |
|---|---|---|
| Registro simples pelo celular ou computador, com localização no mapa e foto. Protocolo para acompanhar. Aviso a cada mudança de situação. Possibilidade de apoiar uma demanda que já existe em vez de abrir outra. | Visão consolidada de todas as demandas em painel e mapa. Fila priorizada por critérios objetivos (risco, apoios, tempo em aberto). Registros padronizados e completos. Histórico para planejar equipes e orçamento por região. | Resolução mais rápida dos problemas do bairro. Mais participação cidadã nas melhorias locais. Transparência sobre o que foi feito e o que está pendente. Mais confiança na gestão pública. |

## 1.3 Cenário atual (As-Is) e cenário futuro (To-Be)

| Dimensão | Cenário atual (As-Is) | Cenário futuro (To-Be) |
|---|---|---|
| Canais de entrada | Vários canais desconectados (telefone, e-mail, redes sociais, presencial) | Plataforma única, PWA responsiva, acessível pelo celular |
| Padronização | Texto livre, sem categoria, localização imprecisa ou ausente | Formulário com categoria, descrição, geolocalização no mapa e foto opcional |
| Duplicidade | O mesmo problema registrado várias vezes, sem vínculo entre os registros | Sistema alerta sobre ocorrências próximas da mesma categoria; o cidadão pode apoiar a existente |
| Priorização | Ordem de chegada ou pressão política e social | Pontuação objetiva por risco da categoria, número de apoios e tempo em aberto |
| Acompanhamento | O cidadão não sabe a situação e precisa ligar de novo para perguntar | Protocolo, linha do tempo de status e notificação automática |
| Gestão | Sem visão consolidada; decisões baseadas em percepção | Painel com mapa, filtros e indicadores por bairro, categoria e status |
| Dados e histórico | Informação espalhada e perdida | Histórico completo e auditável de cada demanda |

## 1.4 Papéis dos usuários

| Papel | Responsabilidades | Permissões principais |
|---|---|---|
| **Cidadão** | Registra demandas com categoria, descrição, localização e foto. Acompanha o andamento pelo protocolo. Apoia demandas já existentes. | Criar ocorrência; consultar as próprias ocorrências e o painel público (sem dados pessoais de terceiros); apoiar ocorrência |
| **Gestor Público** | Analisa e valida as demandas da sua área. Prioriza a fila. Atualiza a situação com justificativa. Indefere duplicadas ou inválidas. Encerra as solicitações resolvidas. | Consultar ocorrências da sua área com dados do solicitante; alterar status e prioridade; registrar justificativa e evidência de resolução |
| **Administrador** | Gerencia usuários e perfis, categorias de demanda (incluindo o peso de risco usado na priorização), áreas de atuação e configurações da plataforma. | Cadastrar e editar usuários, papéis, categorias e parâmetros; consultar trilha de auditoria |

## 1.5 Requisitos funcionais

| ID | Descrição do requisito | Prioridade | Critério de aceite (resumo) |
|---|---|---|---|
| **RF01** | **Registrar ocorrência.** O cidadão autenticado registra uma demanda informando categoria, descrição, localização (marcação no mapa ou GPS do dispositivo) e, opcionalmente, até 3 fotos. | Alta | Ocorrência salva com status "Aberta" e protocolo único devolvido ao cidadão. Campos obrigatórios validados. |
| **RF02** | **Acompanhar ocorrência.** O cidadão consulta suas ocorrências pelo protocolo e vê a linha do tempo de mudanças de status, com data e justificativa. | Alta | Histórico completo e em ordem cronológica. O cidadão só acessa os dados pessoais das próprias ocorrências. |
| **RF03** | **Atualizar situação.** O gestor altera o status seguindo o ciclo Aberta → Em análise → Em execução → Resolvida → Encerrada, ou Indeferida (a partir de Aberta ou Em análise), sempre com justificativa. | Alta | Só transições válidas são aceitas. Toda mudança fica registrada no histórico com autor e data. |
| **RF04** | **Priorizar demanda.** O sistema calcula uma pontuação de prioridade a partir do risco da categoria, do número de apoios e do tempo em aberto. O gestor pode ajustar a prioridade manualmente, com justificativa. | Alta | Fila ordenada pela pontuação. Ajuste manual registrado com justificativa. Sem apoios (RF08), a pontuação usa só o risco da categoria e o tempo em aberto. |
| **RF05** | **Painel consolidado.** O gestor visualiza as demandas em lista e em mapa, com filtros por bairro, categoria, status, prioridade e período, além de indicadores (abertas, em execução, tempo médio de resolução). | Média | Filtros combináveis. Indicadores coerentes com a lista filtrada. |
| **RF06** | **Notificar cidadão.** O sistema avisa o cidadão por e-mail ou push sempre que a situação da sua ocorrência muda. | Média | Notificação enviada após cada mudança de status. Uma falha no envio não impede a mudança de status. |
| **RF07** | **Administrar usuários e categorias.** O administrador gerencia usuários, papéis, áreas de atuação dos gestores e categorias (com peso de risco). | Média | Só o perfil Administrador tem acesso. Alterações registradas em auditoria. |
| **RF08** | **Apoiar ocorrência e sinalizar duplicidade.** Ao registrar, o sistema mostra ocorrências abertas da mesma categoria num raio próximo. O cidadão pode apoiar uma delas em vez de criar outra. | Baixa | Um apoio por cidadão por ocorrência. O apoio aumenta a pontuação do RF04. |

## 1.6 Requisitos não funcionais

Os atributos seguem a nomenclatura da ISO/IEC 25010.

| ID | Descrição do requisito | Atributo de qualidade | Meta mensurável |
|---|---|---|---|
| **RNF01** | Operações de consulta e de registro (sem contar o upload de fotos) devem responder em até **200 ms no percentil 95**, sob carga de **100 requisições por segundo**. | Eficiência de desempenho | p95 ≤ 200 ms sob 100 req/s |
| **RNF02** | A autenticação usa **OAuth2/OpenID Connect** com token **JWT** de curta duração. A autorização é por papel (**RBAC**: Cidadão, Gestor Público, Administrador), verificada em todas as operações da API. Toda comunicação usa **HTTPS**. As entradas são validadas e sanitizadas contra as vulnerabilidades do **OWASP Top 10**. | Segurança (autenticidade, confidencialidade, integridade) | 100% das rotas da API exigem autenticação e verificação de papel. 100% do tráfego em HTTPS. Token de acesso com validade de 15 minutos, renovado por refresh token. |
| **RNF03** | O tratamento de dados pessoais segue a **LGPD**. Coleta mínima (só o necessário para contato e acompanhamento). Dados do solicitante visíveis apenas ao gestor responsável e ao administrador. Painel público sem dados pessoais. Suporte à anonimização dos dados quando o titular pedir. Trilha de auditoria dos acessos. | Segurança (confidencialidade e responsabilização) | 0 campos de dados pessoais nas respostas do painel público. 100% dos acessos a dados do solicitante na trilha de auditoria. |
| **RNF04** | A plataforma deve ter **disponibilidade mensal de 99,5%**, com **backup diário** dos dados e procedimento de recuperação documentado (**RPO ≤ 24 h, RTO ≤ 4 h**). | Confiabilidade (disponibilidade, recuperabilidade) | 99,5% ao mês. RPO ≤ 24 h. RTO ≤ 4 h. |
| **RNF05** | A interface deve ser **responsiva** (uso prioritário pelo celular) e seguir a **WCAG 2.1 nível AA**. O registro de uma ocorrência deve levar no máximo **3 passos**. | Usabilidade (acessibilidade, operabilidade) | WCAG 2.1 AA. Registro em até 3 passos. |
| **RNF06** | Regras de negócio (priorização, transições de status, validações) devem ser **independentes de framework, banco e interface**, com **cobertura de testes automatizados ≥ 80%** nas camadas de domínio e de aplicação. | Manutenibilidade (modularidade, testabilidade) | Cobertura ≥ 80% em domínio e aplicação. |

**Requisitos atendidos:** RF01, RF02, RF03, RF04, RF05, RF06, RF07, RF08, RNF01, RNF02, RNF03, RNF04, RNF05, RNF06