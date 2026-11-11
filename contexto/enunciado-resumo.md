# Enunciado – GS2026.2 · Neighborhood Action Dashboard (resumo da equipe)

## Problema
Plataforma digital para registrar, organizar e acompanhar demandas urbanas de bairros e comunidades:
iluminação pública, acessibilidade, praças e parques, resíduos, buracos em vias, sinalização e
manutenção de equipamentos públicos. Hoje as demandas chegam por canais diferentes, sem padronização,
o que dificulta acompanhar, priorizar e resolver.

Papéis: Cidadão (registra, informa localização, acompanha) · Gestor Público (analisa, prioriza,
atualiza situação, encerra) · Administrador (usuários, categorias, configurações).

## Regras gerais
- Foco em arquitetura e documentação, não em implementação.
- Entrega: um único PDF. Duração de referência: até 4 h. Individual, dupla ou trio.
- Uma única abordagem arquitetural e uma única visão (C4 ou 4+1) – nunca misturar.
- Justificar todas as decisões técnicas com conceitos da disciplina.

## Etapa 1 – Visão do Negócio (2,0)
Objetivo, problema, público-alvo, benefícios esperados; benefícios para cidadãos, gestores e comunidade;
As-Is × To-Be; papéis e responsabilidades; mínimo 5 RF (com prioridade Alta/Média/Baixa) e
4 RNF (com atributo de qualidade).

## Etapa 2 – Arquitetura da Solução (2,0)
Escolher entre Camadas, Hexagonal, Clean ou Onion. Justificar: motivação, benefícios, limitações,
alternativas descartadas. Perguntas norteadoras:
- Quais atributos de qualidade são mais críticos?
- Como as regras de negócio ficam separadas de interface, banco e integrações?
- Que complexidade extra a abordagem traz para o tamanho da equipe e o prazo?
- Por que cada alternativa foi descartada?
- Como a escolha se conecta às visões (C4 ou 4+1)?

## Etapa 3 – Visões Arquiteturais (2,5)
Opção escolhida: 4+1 (Kruchten). Entregar Visão Lógica (entidades e relacionamentos, ex.: Ocorrência,
Usuário, Categoria, Status), Visão de Desenvolvimento (módulos, pacotes, camadas e dependências),
Visão de Processo (fluxos, comunicação, concorrência, desempenho) e Cenário +1 (caso de uso que integra
as visões). Explicação resumida do modelo.

## Etapa 4 – Cenário Crítico e Sequência (1,5)
Um fluxo crítico (escolhido: Registrar ocorrência). Participantes, mensagens numeradas, fluxo principal
e fluxos alternativos (ex.: dados inválidos, falha ao salvar). Tabela # | De | Para | Mensagem
+ diagrama de sequência.

## Etapa 5 – ADR (1,5)
Um ADR-001: Status/Data (Proposta, Aceita ou Substituída), Contexto, Decisão ("Decidimos adotar…"),
Alternativas consideradas com motivo do descarte, Consequências positivas e negativas.

## Etapa 6 – Qualidade e Testes (0,5)
Pirâmide de testes com exemplos do domínio: unitários (priorização, validação de campos), integração
(API de ocorrências gravando/consultando no banco), E2E (cidadão registra e gestor atualiza até encerrar).
Definition of Done preenchível com 8 critérios: requisito e critérios de aceite validados; testes
unitários aprovados; testes de integração aprovados; fluxo principal em E2E; código revisado por outro
integrante; decisão em ADR quando aplicável; sem defeitos críticos abertos; documentação atualizada.

## Checklist final do PDF
Capa · Integrantes · Documento de Visão · Arquitetura da Solução · C4 ou 4+1 · Diagrama de Sequência ·
ADR · Estratégia de Testes · Referências