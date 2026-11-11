---
description: Gerar a Etapa 3 – Visões 4+1 (Lógica, Desenvolvimento, Processo, Cenário +1)
---
Leia [decisões](../../contexto/decisoes.md), [visão](../../docs/01-documento-de-visao.md)
e [arquitetura](../../docs/02-arquitetura-da-solucao.md).

Gere uma visão por vez, na ordem abaixo, aguardando meu ok entre elas. Cada uma com breve explicação
do que a visão representa (Kruchten) + diagrama Mermaid pronto para renderizar + tabela de elementos.

1. `03.1-visao-logica.md` – classDiagram com as entidades canônicas, atributos-chave, multiplicidades,
   e stateDiagram-v2 do ciclo de status (RF03).
2. `03.2-visao-desenvolvimento.md` – organização em módulos (monólito modular: ocorrencias, priorizacao,
   notificacoes, identidade, administracao) × camadas da Clean Architecture; diagrama de pacotes
   (flowchart) mostrando dependências apontando para o domínio.
3. `03.3-visao-processo.md` – fluxos de execução, comunicação síncrona (HTTPS/REST) e assíncrona
   (notificação por fila/eventos internos), concorrência (ex.: apoios simultâneos, mudança de status
   concorrente) e desempenho (RNF01, RNF04); incluir sistemas externos.
4. `03.4-cenario-mais1.md` – caso de uso Registrar ocorrência; tabela mostrando como cada visão o sustenta.

Não incluir Visão Física como entrega obrigatória (pode citar em uma linha). Nada de C4.
Ao final: checagem da rubrica da Etapa 3 e atualização da rastreabilidade.
Siga o [guia do especialista](../../contexto/guia-especialista.md) e o modo de trabalho do copilot-instructions.md.
