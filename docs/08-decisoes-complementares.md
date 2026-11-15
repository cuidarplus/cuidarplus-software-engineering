# 8. Registro de decisões complementares

Esta seção reúne as decisões fechadas D1 a D9 e indica em que parte do documento cada uma é aplicada.

| ID | Decisão | Escolha | Onde é aplicada |
|---|---|---|---|
| D1 | Abordagem arquitetural | Clean Architecture | docs/02 |
| D2 | Visão arquitetural | 4+1 (Lógica, Desenvolvimento, Processo, +1) | docs/03 |
| D3 | Fluxo crítico | Registrar ocorrência (RF01) | docs/04 |
| D4 | ADR-001 | Monólito modular | docs/05-adr |
| D5 | Stack tecnológica | React + TypeScript (PWA); Java LTS + Spring Boot + Spring Modulith; PostgreSQL + PostGIS (Spring Data JPA + Hibernate Spatial); eventos de domínio com registro de publicação (outbox); armazenamento de objetos S3-compatível | docs/02 e docs/03.3 |
| D6 | Aplicação da Clean por módulo | Completa em ocorrencias; priorizacao só com domínio (cálculo puro, D7); enxuta em administracao e identidade; notificacoes reativa a eventos | docs/02 e docs/03.2 |
| D7 | Fronteiras entre módulos | priorizacao é cálculo puro sem dependências; ocorrencias é dona de Apoio, da pontuação gravada e do recálculo agendado; administracao é dona de Categoria e AreaAtuacao; notificacoes reage a eventos | docs/03.2 e docs/03.3 |
| D8 | Duplicidade e upload | Consulta de duplicidade prévia e não bloqueante (A3); upload de foto direto ao armazenamento por URL temporária emitida pela API | docs/03.3 e docs/04 |
| D9 | Emissão de tokens | gov.br autentica via OIDC; identidade emite JWT próprio (15 min, com papéis) e refresh token | docs/03.3 e docs/04 |

**Requisitos atendidos:** RF01, RF04, RF06, RF08, RNF01, RNF02, RNF06
