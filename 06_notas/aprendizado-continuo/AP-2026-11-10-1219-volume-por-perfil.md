# AP-2026-11-10-1219 — O volume real da base muda o que é testável; perguntar o perfil antes

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T6.01 (SPEC-1-006, CA-1-24 e CA-1-26)
- Sinal: montei um roteiro de teste de 5 itens assumindo que a base tinha mais de 25
  oportunidades — número necessário para o controle de paginação aparecer. O cliente informou
  "minha base só tem 3". Replanejei o teste e cheguei a propor criar 27 oportunidades artificiais.
  Em seguida o cliente corrigiu: **eram 3 apenas porque ele estava logado como colaborador Saúde;
  como administrador via 8**. O dado original não estava errado — estava incompleto por perfil.
- Evidência: `.adapta-cliente/estado-atual.md` (registro de 11:12 substituído pelo de 12:13);
  `04_fase-atual/fase.md` T6.01 concluída em 10/11.
- Regra reutilizável: antes de escrever um roteiro de teste que dependa de volume, **verificar se o
  número observado é o total ou apenas o que aquele perfil vê**. Com RLS, "quantos registros
  existem" tem respostas diferentes por usuário — e o número menor pode ser a prova de segurança,
  não uma limitação do ambiente.
- Quando aplicar: toda task cuja prova dependa de quantidade de registros (paginação, CSV, painel
  com contagens, filtros) e todo teste de visibilidade por perfil.
- Quando não aplicar: quando o dado independe de permissão (catálogo, configuração do sistema).
- Confiança: alta — a diferença 3 × 8 apareceu na mesma sessão e virou o teste mais forte da task
  (CA-1-26: o CSV do colaborador não traz o que ele não pode ver).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.

## Desdobramento útil

A confusão produziu o melhor teste do ciclo: comparar **lista e CSV nos dois perfis**. O item que
 eu havia classificado como "não testável" virou a prova central. Registrar isso evita que, numa
próxima vez, eu trate um número baixo como limitação antes de perguntar em qual perfil ele foi
observado.
