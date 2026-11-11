# cuidarplus-software-engineering

Entregável da disciplina de Engenharia de Software (FIAP, Global Solution 2026-2), com a documentação de arquitetura da solução CuidarPlus.

## Estrutura

```
.github/        instruções globais do Copilot e prompts por seção
contexto/       enunciado resumido, decisões fechadas e rastreabilidade RF/RNF
docs/           entregável, um arquivo por seção do enunciado
  05-adr/       registros de decisões arquiteturais
referencias/    materiais de apoio (fora do Git, ver .gitignore)
```

## Como usar

Os prompts em `.github/prompts/` são executados na ordem numérica (`01` a `06`) e a revisão final é feita pelo `99-checklist-final`. Antes de começar, leia `contexto/decisoes.md`, que é a fonte de verdade das escolhas do projeto.