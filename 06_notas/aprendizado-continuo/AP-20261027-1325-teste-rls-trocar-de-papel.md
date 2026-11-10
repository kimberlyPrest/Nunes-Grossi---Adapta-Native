# AP-20261027-1325 — Teste de RLS exige trocar de papel

**Data:** 27/10/2026 · **Task:** T2.02 · **Evidência:** `06_notas/debug/2026-10-27-t202-delete-silencioso-rls.md`

## Sinal reutilizável

No Supabase, o SQL editor executa como `postgres`, **dono** das tabelas de `public`. No PostgreSQL, o
dono da tabela **ignora a RLS**, a menos que a tabela tenha `FORCE ROW LEVEL SECURITY`
([PostgreSQL 18 §5.9](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)). O app nunca usa
esse caminho: fala pelo PostgREST como `authenticated`.

Consequência prática: **qualquer verificação de RLS feita no editor sem trocar de papel mede o
oposto do que se quer saber** — dá "funcionou" mesmo quando a política está correta e "apagou" mesmo
quando o app não consegue apagar.

## Regra para os próximos testes

```sql
BEGIN;
SET LOCAL ROLE authenticated;          -- o papel real do app
-- ... teste ...
ROLLBACK;
```

Limitações do editor que também custaram tentativas:
1. Só o **último result set** aparece — consolidar em uma única consulta (CTE modificadora, sem
   `CREATE TEMP TABLE`, que dispara aviso de "table without RLS").
2. RLS **filtra em silêncio**: bloqueio de DELETE/UPDATE não gera erro, gera 0 linhas. Testar por
   contagem ou por linhas afetadas (`RETURNING`), nunca esperando mensagem de erro.

## Onde isso aparece de novo

Toda task com prova de permissão: T2.03 (download negado de documentos), T2.04 (menus/rotas por
perfil + regressão), T4.02 (abertura de oportunidade), T5.05 (notificações). O mesmo cuidado vale para
conferir se uma política `FOR ALL` reabriu DELETE — a view `verificacao_regras_de_acesso` hoje **não
pega** `FOR ALL TO authenticated`.
