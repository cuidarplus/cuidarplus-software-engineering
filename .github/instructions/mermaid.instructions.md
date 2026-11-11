---
applyTo: "docs/**/*.md"
---
# Regras de Mermaid (evitar erro de renderização)

- Sempre em bloco ```mermaid. Tipos permitidos: classDiagram, stateDiagram-v2, flowchart, sequenceDiagram.
  Proibido: C4Context, C4Container e qualquer sintaxe C4.
- IDs sem acento, espaço ou hífen (Ocorrencia, EmAnalise, ApiRest). Rótulos com acento vão entre aspas:
  `A["Gestor Público"]`.
- Não usar parênteses, dois-pontos, ponto e vírgula ou barra em rótulos sem aspas.
- stateDiagram-v2: estados com espaço via alias – `state "Em análise" as EmAnalise`.
- classDiagram: multiplicidade entre aspas – `Ocorrencia "1" --> "0..*" HistoricoStatus : possui`.
- sequenceDiagram: começar com `autonumber`; `participant API as API REST`; usar `alt`/`else`/`end` e `opt`/`end`.
- flowchart: `flowchart TB` ou `LR`; agrupar com `subgraph id["Rótulo"]` … `end`.
- Quebra de linha em rótulo: `<br/>`.
- Máximo de ~20 elementos por diagrama; se passar disso, dividir em dois.
- Depois de gerar, revisar a sintaxe linha a linha; o diagrama precisa renderizar em mermaid.live.
