# AP-20261030-1957 — Fechar oportunidade ao "não sobrar etapa pendente" erra quando a etapa atual ainda corre

- **Data**: 2026-10-30T19:57:00-03:00
- **Origem**: T5.03a (Central do Fluxo Comercial, N&G)
- **Tipo**: SQL / regra de fechamento
- **Evidência**: na primeira versão de `confirmar_ganho_oportunidade` eu encerrava a oportunidade quando
  `NOT EXISTS (etapa com situacao <> 'concluida' E id <> etapa_atual)`. No cenário real da J6 — ganho
  na etapa 11, etapa 12 pendente — as etapas 1..11 estão concluídas e a 12 pendente; o filtro excluía a
  12 por ser "a atual", então a oportunidade virava "Concluída" na 11. O teste da J6 real pegou
  (`confirmar ganho nao encerra com etapa 12 pendente` -> obtido "Concluída").

## Sinal reutilizável

Para decidir "posso fechar isto?", a condição correta é **todas as etapas concluídas** — sem excluir a
atual do filtro. A etapa atual é justamente a que ainda pode estar pendente. Ao escrever um `NOT EXISTS`
de conclusão, conferir se o predicado não está isentando exatamente o registro que ainda falta.

## Como aplicar

1. Regra de fechamento expressa como `NOT EXISTS (... AND situacao <> 'concluida')`, sem isenções.
2. Testar com o fluxo REAL (J6: ganho na 11, etapa 12 depois), não só com um fluxo de uma etapa.
3. Incluir sempre um cenário "última etapa é a de ganho" separado do "etapa de ganho no meio" — os dois
   exigem comportamentos opostos (encerrar vs. continuar).
