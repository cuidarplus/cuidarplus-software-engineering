---
description: Gerar a Etapa 6 – Estratégia de Testes e DoD
---
Leia [visão](../../docs/01-documento-de-visao.md), [arquitetura](../../docs/02-arquitetura-da-solucao.md)
e [sequência](../../docs/04-diagrama-sequencia.md).

Gere `docs/06-estrategia-de-testes.md`:
1. Pirâmide (diagrama Mermaid) e justificativa da proporção, ligada à Clean Architecture (RNF06).
2. Tabela por nível com exemplos do domínio citando RFs: unitários (RF04 priorização, RF03 transições,
   RF01 validação), integração (API de ocorrências + banco; envio de notificação RF06), E2E
   (cidadão registra → gestor atualiza → encerra).
3. Definition of Done preenchível com os 8 critérios do enunciado + coluna de evidência.
Sem ferramentas de pipeline. Ao final: checagem da rubrica da Etapa 6 e atualização da rastreabilidade.
Siga o [guia do especialista](../../contexto/guia-especialista.md) e o modo de trabalho do copilot-instructions.md.
