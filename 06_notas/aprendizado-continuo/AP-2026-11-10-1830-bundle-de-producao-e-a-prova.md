# AP-2026-11-10-1830 — Correção "pronta" mas não publicada: o bundle de produção é a única fonte

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T6.02 · SPEC-1-006
- Sinal: o `src/services/fluxos.ts` corrigido existia no clone local e no working tree do Skip desde as 15:47, com prova 8/8 e build OK, mas os registros do projeto diziam "não publicado". Verificação no bundle servido mostrou o contrário em parte: a correção do service **já estava em produção** desde a v0.0.56 (`0d66112`), publicada fora daquele ciclo, e o que faltava era só a tela (`Quadro.tsx`). A conclusão anterior estava errada em dois sentidos ao mesmo tempo: subestimava o que estava no ar e não distinguia "arquivo alterado no working tree" de "arquivo commitado/buildado/publicado".
- Evidência: `/tmp/p.js` (bundle `index-DlaqEsVQ.js`, servido antes deste ciclo) já continha `order("is_generico",{ascending:true})`; `skip_project_status` mostrava `lastPublishedRef` = HEAD (`0d66112`) antes da T6.02; o `fluxos.ts` no espelho GitHub seguia `ascending:false` porque o espelho está parado em 16/10.
- Regra reutilizável: antes de afirmar "está publicado" ou "não está publicado", baixar o bundle que a produção serve e procurar uma string de UI que só exista na versão nova. Não usar como prova: working tree do Skip, `git log` do clone, espelho GitHub, `lastProdBuildRef` do `.skip.config.json`, nem a versão do `package.json`.
- Quando aplicar: sempre que o estado de uma mudança em produção for citado em relatório, análise de task ou conclusão.
- Quando não aplicar: quando a mudança é só de banco (aí a prova é `pg_trigger`/consulta de dados, não bundle) ou quando a task não toca frontend.
- Confiança: alta — duas fontes independentes (bundle servido e `skip_project_status`) concordaram depois da checagem, e a divergência tinha causa identificada.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
