# AP-20261028-1100 — Checklist tem dois renderizadores: mudança precisa dos dois

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T2.04 · SPEC-1-002 (relacionado a T2.03 / CA-1-06)
- Sinal: o mesmo dado de checklist é renderizado por **dois** componentes, em momentos diferentes
  do ciclo de vida da etapa. Alterar um não altera o outro, e o defeito só aparece quando a etapa
  muda de estado.
- Evidência: `06_notas/debug/debug-2026-10-27-t204-documentos-etapa-concluida.md`. A T2.03 entregou
  download por link assinado e foi aprovada no checklist editável (`ChecklistEtapaAtual`); o
  checklist somente leitura (`TrilhaEtapas`), usado em etapas concluídas, continuou exibindo apenas
  `arquivo_nome` — todo documento de etapa fechada ficou inalcançável, inclusive para administrador.
  Corrigido em `0628086`.
- Regra reutilizável: antes de fechar qualquer mudança em checklist de etapa, procurar **todos** os
  componentes que renderizam aquele item (`grep` pelo tipo do item, ex. `'Documento'`) e aplicar a
  mudança em cada um — ou confirmar por escrito por que um deles não deve receber a alteração.
- Quando aplicar: qualquer alteração em `ChecklistEtapaAtual`, `TrilhaEtapas` ou novo componente que
  renderize `OportunidadeChecklistItem` — campos, permissões, ações, formatos de exibição.
- Quando não aplicar: quando o componente é intencionalmente somente leitura e a ação não faz
  sentido nele — mas visualização e download **fazem** sentido, porque a ausência de saída deixa o
  dado órfão; nesse caso a diferença precisa ser deliberada e registrada.
- Confiança: alta — a falha foi observada em teste humano, a causa raiz foi demonstrada e a
  correção foi validada pelo cliente.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
