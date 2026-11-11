---
description: Gerar a Etapa 4 – Diagrama de Sequência de Registrar ocorrência
---
Leia [decisões](../../contexto/decisoes.md), [visão de desenvolvimento](../../docs/03-visoes-4mais1/03.2-visao-desenvolvimento.md)
e [visão de processo](../../docs/03-visoes-4mais1/03.3-visao-processo.md).

Gere `docs/04-diagrama-sequencia.md`:
1. Participantes coerentes com a arquitetura (Cidadão, Web App, API/Controller, Caso de Uso
   RegistrarOcorrencia, Entidade/Domínio, Repositório, Banco, Serviço de Mapas, Armazenamento de fotos,
   Notificação). Explique cada um em 1 linha.
2. Tabela `# | De | Para | Mensagem` com fluxo principal numerado.
3. Fluxos alternativos: A1 dados inválidos; A2 localização fora da área atendida; A3 possível duplicidade
   (RF08 – oferecer apoio); A4 token expirado/sem permissão (RNF02); A5 falha ao salvar.
4. Diagrama `sequenceDiagram` Mermaid com `autonumber` e blocos `alt`/`opt`.
Ao final: checagem da rubrica da Etapa 4 e atualização da rastreabilidade.
Siga o [guia do especialista](../../contexto/guia-especialista.md) e o modo de trabalho do copilot-instructions.md.
