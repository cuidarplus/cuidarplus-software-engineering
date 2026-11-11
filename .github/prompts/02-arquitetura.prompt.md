---
description: Gerar a Etapa 2 – Arquitetura da Solução (Clean Architecture)
---
Leia [decisões](../../contexto/decisoes.md) e [documento de visão](../../docs/01-documento-de-visao.md).

Antes de escrever, pergunte-me a stack tecnológica (decisão D5) se ainda estiver "A DEFINIR",
sugerindo uma opção simples com justificativa.

Gere `docs/02-arquitetura-da-solucao.md` contendo:
1. Quadro de escolha com Clean Architecture marcada (Camadas, Hexagonal, Onion desmarcadas).
2. Descrição dos círculos (Entidades, Casos de Uso, Adaptadores de Interface, Frameworks/Drivers)
   aplicados ao domínio: onde ficam priorização (RF04), transições de status (RF03), validações (RF01).
3. Regra de dependência e inversão de dependência (portas/repositórios) explicadas com exemplos do domínio.
4. Quadro: Motivação · Benefícios · Limitações · Alternativas descartadas (Camadas, Hexagonal, Onion – motivo de cada).
5. Respostas explícitas às 5 perguntas norteadoras, citando RNF02, RNF06 e RNF01.
6. Parágrafo ligando a abordagem às visões 4+1 (Desenvolvimento = círculos; Lógica = entidades; Processo = execução).

Finalize com checagem da rubrica da Etapa 2 e proposta de atualização da rastreabilidade.
Siga o [guia do especialista](../../contexto/guia-especialista.md) e o modo de trabalho do copilot-instructions.md.
