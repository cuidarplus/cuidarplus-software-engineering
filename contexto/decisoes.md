# Decisões fechadas

| # | Decisão | Escolha | Data | Onde é justificada |
|---|---|---|---|---|
| D1 | Abordagem arquitetural | Clean Architecture | 11/11/2026 | docs/02 |
| D2 | Visão arquitetural | 4+1 (Lógica, Desenvolvimento, Processo, +1) | 11/11/2026 | docs/03 |
| D3 | Fluxo crítico | Registrar ocorrência (RF01) | 11/11/2026 | docs/04 |
| D4 | ADR-001 | Monólito modular | 11/11/2026 | docs/05-adr |
| D5 | Stack tecnológica | React + TypeScript (PWA) · Java LTS + Spring Boot + Spring Modulith · PostgreSQL + PostGIS (Spring Data JPA + Hibernate Spatial) · eventos de domínio com registro de publicação (outbox) · armazenamento de objetos S3-compatível | 11/11/2026 | docs/02 e docs/03.3 |

## Justificativa resumida da stack (D5)

| Camada | Escolha | Por quê | Alternativa descartada |
|---|---|---|---|
| Front-end | React + TypeScript, PWA responsiva | Um código para celular e desktop (RNF05) | App nativo: dobra o esforço |
| Back-end | Java LTS + Spring Boot + Spring Modulith | Ecossistema maduro para APIs REST; Spring Security cobre OAuth2/OIDC + JWT + RBAC (RNF02); Modulith verifica fronteiras dos módulos do monólito (ADR-001) | NestJS: sem verificador nativo de fronteiras de módulo e ecossistema de segurança menos consolidado |
| Persistência | PostgreSQL + PostGIS via Spring Data JPA + Hibernate Spatial | Consultas por raio/bairro (RF05, RF08), ACID, versionamento otimista | MongoDB: integridade relacional mais fraca |
| Notificações | Eventos de domínio + registro de publicação (outbox) + listener assíncrono | Falha no envio não bloqueia a mudança de status (RF06); sem broker (KISS) | RabbitMQ/Kafka: sobreengenharia para o RNF01 |
| Arquivos | Armazenamento de objetos S3-compatível | Fotos fora do banco (RNF04) | Binário no banco |

Limitação assumida: entidades de domínio são classes Java puras, separadas das entidades JPA, com mapper
entre elas – mais código, em troca da independência de framework exigida pelo RNF06.

## Metas numéricas adotadas (metas de projeto, não dados de mercado)
- Desempenho: p95 ≤ 200 ms sob 100 req/s (README da equipe)
- Disponibilidade: 99,5% mensal; RPO ≤ 24 h; RTO ≤ 4 h
- Cobertura: ≥ 80% em domínio e aplicação
- Usabilidade: WCAG 2.1 AA; registro em até 3 passos; até 3 fotos por ocorrência
- Segurança: token de acesso com validade de 15 minutos, renovado por refresh token (RNF02)

## Integrantes
- José Ribeiro dos Santos Neto – RM 567692
- Danilo Roberto Anterio dos Santos – RM 566966
