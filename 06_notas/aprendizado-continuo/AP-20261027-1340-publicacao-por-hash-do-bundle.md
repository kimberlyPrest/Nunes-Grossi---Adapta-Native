# AP-20261027-1340 — Provar que a correção está no ar pelo hash do bundle

**Data:** 27/10/2026 · **Task:** T2.02 · **Sintoma recorrente:** código corrigido no GitHub mas teste humano falhando por estar numa versão antiga (aconteceu na T2.01, duas vezes, e de novo na T2.02).

## Sinal reutilizável

O `.skip.config.json` (`lastProdBuildRef`, `lastPublishedRef`) e a versão do `package.json` **não
provam** que o site em produção está com o código novo — são registros do pipeline, e neste projeto já
apareceram apontando para o build anterior mesmo depois de publicar.

Prova direta, em dois passos:

```bash
# 1. Hash do JS servido em produção
curl -s https://central-do-fluxo-comercial-ng-bf9f0.goskip.app | grep -oE 'assets/index-[^"]+\.js'
# 2. Gerar o build do commit atual e comparar o nome do arquivo
npx vite build && ls dist/assets/*.js
```

Se o hash bater, o que está no ar é **exatamente** aquele commit. Verificado na T2.02: o bundle
publicado (`index-Dq_oFxR1.js`) saiu idêntico ao build gerado de `origin/main`.

Complemento sem gerar build: procurar no JS publicado uma string nova da correção
(`curl … assets/index-<hash>.js | grep "texto da mensagem"`). Serve como sinal rápido, mas o hash é a
prova completa.

## Quando aplicar

Antes de pedir teste humano em qualquer task deste projeto — e sempre que o teste falhar de um jeito
que não bate com o código lido.

## Limite

O hash prova o **frontend**. Não prova migração aplicada no banco: isso exige consulta própria
(ex.: `SELECT tgname FROM pg_trigger WHERE tgname = 'trg_protege_situacao_cliente';`).
