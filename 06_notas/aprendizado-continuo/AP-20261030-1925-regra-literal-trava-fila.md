# AP-20261030-1925 — Regra de permissão lida ao pé da letra pode travar um fluxo de fila

- **Data**: 2026-10-30T19:25:00-03:00
- **Origem**: T5.02 (Central do Fluxo Comercial, N&G) — D-23
- **Tipo**: requisito / modelagem de permissão
- **Evidência**: a SPEC-1-005 regra 3 diz "só o responsável da etapa atual (ou administrador/gestor da equipe) conclui". Implementado literalmente, o teste humano travou: a etapa 12 da J6 estava assumida pela Louise e o Pedro, colega da mesma equipe/função, não podia assumir ("já assumida") nem concluir ("apenas o responsável"). A fila ficou sem saída enquanto a pessoa estivesse ausente.

## Sinal reutilizável

Quando a etapa é de **fila** (trabalho destinado a uma equipe + função, não a uma pessoa), a
permissão de movimentar deve ser da **equipe + função**, não do indivíduo que assumiu. "Assumir"
é marcador de intenção visível aos colegas, não reserva exclusiva. Reservar a etapa a uma pessoa
cria um ponto único de falha: férias, desligamento ou simples ausência travam o processo, e o
usuário não tem saída pela interface.

## Como aplicar

1. Ao implementar regra de permissão da SPEC, separar **etapa de pessoa** (responsável comercial)
   de **etapa de fila** (equipe + função) antes de escrever a validação.
2. Em etapa de fila, validar `pertence à equipe AND função confere`; não exigir `responsavel_usuario_id`.
3. Testar sempre o caso "etapa já assumida por outra pessoa, colega da mesma equipe tenta agir" —
   é o cenário que revela a trava.
4. Se a SPEC for omissa ou ambígua nesse ponto, registrar DÚVIDA antes de implementar: é decisão de
   negócio, não de código.
