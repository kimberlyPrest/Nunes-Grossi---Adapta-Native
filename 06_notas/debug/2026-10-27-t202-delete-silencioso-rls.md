# Debug — T2.02: teste de exclusão inconclusivo (RLS filtra DELETE em silêncio)

**Data:** 27/10/2026 · **Task:** T2.02 · **SPEC:** SPEC-1-002 · **Relatado por:** Pedro (print do SQL editor)

## Sintoma

Rodei o script `BEGIN; DELETE ...; ROLLBACK;` no SQL editor do Supabase: resultado
**"Success. No rows returned"** — sem erro de permissão, ao contrário do que eu previ.

## Causa (erro de premissa minha, não do sistema)

No Postgres, quando a RLS bloqueia um DELETE **não há erro**: as linhas simplesmente não casam com a
política e a instrução afeta **0 linhas**. O SQL editor do Supabase mostra "Success. No rows returned"
tanto para "apagou 0 porque o banco bloqueou" quanto para "não havia linha". Ou seja: minha instrução
de teste estava **errada** — o erro de permissão que eu prometi nunca apareceria, nem se o banco
estivesse bloqueando corretamente, nem se estivesse liberando.

## Impacto nos dados

**Nenhum.** A transação terminou em `ROLLBACK`; nada foi apagado em qualquer cenário.

## Teste correto (contagem antes/depois dentro da transação)

A única prova confiável é comparar contagens dentro da mesma transação, antes do `ROLLBACK`:
se o número não muda, o DELETE foi bloqueado pelo banco.

```sql
BEGIN;
SELECT 'antes' AS momento, count(*) AS contatos FROM public.clientes_contatos;
SELECT 'antes' AS momento, count(*) AS oportunidades FROM public.oportunidades;
DELETE FROM public.clientes_contatos WHERE id = (SELECT id FROM public.clientes_contatos LIMIT 1);
DELETE FROM public.oportunidades WHERE id = (SELECT id FROM public.oportunidades LIMIT 1);
SELECT 'depois' AS momento, count(*) AS contatos FROM public.clientes_contatos;
SELECT 'depois' AS momento, count(*) AS oportunidades FROM public.oportunidades;
ROLLBACK;
```

Prova complementar independente (só leitura): listar as políticas de DELETE existentes.

```sql
SELECT tablename, policyname, cmd
FROM pg_policies
WHERE schemaname = 'public' AND cmd = 'DELETE'
ORDER BY tablename;
```

Esperado: nenhuma linha para as tabelas do domínio (clientes, oportunidades, contatos, interações,
documentos, catálogo, fluxos).

## Lição

RLS no Postgres **filtra em silêncio**, nunca levanta exceção em DELETE/UPDATE/SELECT. Qualquer teste
de "o banco bloqueia" precisa medir o **efeito** (linhas afetadas / contagem), não esperar mensagem
de erro. É a mesma raiz do bug do arquivamento silencioso: o app confundia "sem erro" com "deu certo".

## Segundo teste — também inconclusivo (limitação do editor)

O script com vários `SELECT` mostrou **uma única linha** (`depois | oportunidades | 5`): o SQL editor do
Supabase exibe **somente o resultado da ÚLTIMA instrução**. As contagens "antes" foram executadas mas
não ficaram visíveis, então a comparação — que era o ponto do teste — não pôde ser lida.

## Teste definitivo — um único resultado, com tudo o que importa

Registrar tudo numa tabela temporária e devolver **uma só** grade no fim:

```sql
BEGIN;
CREATE TEMP TABLE _t202 (ordem int, etapa text, tabela text, qtd bigint);

INSERT INTO _t202 SELECT 1, 'antes',    'oportunidades',     count(*) FROM public.oportunidades;
INSERT INTO _t202 SELECT 1, 'antes',    'clientes_contatos', count(*) FROM public.clientes_contatos;

WITH d AS (DELETE FROM public.oportunidades
            WHERE id IN (SELECT id FROM public.oportunidades LIMIT 1) RETURNING 1)
INSERT INTO _t202 SELECT 2, 'apagadas', 'oportunidades',     count(*) FROM d;

WITH d AS (DELETE FROM public.clientes_contatos
            WHERE id IN (SELECT id FROM public.clientes_contatos LIMIT 1) RETURNING 1)
INSERT INTO _t202 SELECT 2, 'apagadas', 'clientes_contatos', count(*) FROM d;

INSERT INTO _t202 SELECT 3, 'depois',   'oportunidades',     count(*) FROM public.oportunidades;
INSERT INTO _t202 SELECT 3, 'depois',   'clientes_contatos', count(*) FROM public.clientes_contatos;

SELECT etapa, tabela, qtd FROM _t202 ORDER BY ordem, tabela;
ROLLBACK;
```

Leitura: `apagadas = 0` nas duas linhas **e** `antes = depois` → banco bloqueia (CA-1-07 OK).

Prova independente, só leitura:

```sql
SELECT tablename, policyname, cmd FROM pg_policies
WHERE schemaname = 'public' AND cmd = 'DELETE' ORDER BY tablename;
```

## Lição 2

O SQL editor do Supabase exibe **somente o resultado da ÚLTIMA instrução**. Qualquer verificação precisa
terminar numa única consulta consolidada (tabela temporária + um `SELECT` final), nunca em comparações
espalhadas por vários `SELECT`.

## Terceiro teste — bloqueado por aviso do editor (CREATE TABLE sem RLS)

O editor do Supabase abriu o modal "Potential issue detected: creates a table without enabling Row
Level Security" por causa do `CREATE TEMP TABLE`. O aviso **não se aplica** aqui (tabela temporária de
sessão, dentro de transação terminada em `ROLLBACK`, nunca exposta via PostgREST), mas o teste ficou
travado por um alerta legítimo do editor — sinal de que o caminho estava complicado demais.

## Teste definitivo (v3) — sem tabela temporária, uma única grade

O mesmo resultado com **CTEs que modificam dados** (permitido no Postgres), sem criar objeto nenhum:

```sql
BEGIN;

WITH n_o AS (SELECT count(*) AS total FROM public.oportunidades),
     n_c AS (SELECT count(*) AS total FROM public.clientes_contatos),
     t_o AS (SELECT id FROM public.oportunidades LIMIT 1),
     t_c AS (SELECT id FROM public.clientes_contatos LIMIT 1),
     del_o AS (DELETE FROM public.oportunidades
                WHERE id IN (SELECT id FROM t_o) RETURNING 1),
     del_c AS (DELETE FROM public.clientes_contatos
                WHERE id IN (SELECT id FROM t_c) RETURNING 1)
SELECT 'oportunidades' AS tabela, (SELECT total FROM n_o) AS antes,
       (SELECT count(*) FROM del_o) AS apagadas
UNION ALL
SELECT 'clientes_contatos', (SELECT total FROM n_c),
       (SELECT count(*) FROM del_c);

ROLLBACK;
```

Todas as CTEs enxergam o mesmo snapshot, então `antes` é a contagem pré-DELETE e `apagadas` é o que o
banco de fato removeu — exatamente a comparação que faltava, em **uma só grade** e sem aviso do editor.

## Lição 3

Aviso de "table without RLS" no editor do Supabase: só desligar quando for **tabela temporária dentro
de transação com ROLLBACK** — e mesmo assim, preferir um teste que não crie objeto (CTE modificadora),
porque um modal de segurança treina o operador a clicar "Run without RLS" no automático.

## Quarto teste — `apagadas = 1`: ambiente de teste diferente do app, não falha de RLS

Resultado do teste v3: `oportunidades antes=6, apagadas=1` · `clientes_contatos antes=1, apagadas=1`.

Isso **não** indica que o app permite exclusão. O SQL editor do Supabase roda com o papel **dono das
tabelas** (`postgres`), e no Postgres a RLS **não se aplica ao dono da tabela**, salvo
`FORCE ROW LEVEL SECURITY`. Verificado: nenhuma migração usa FORCE RLS.

Logo o DELETE do editor sempre passa — era isso que os três testes anteriores estavam medindo, sem
que eu percebesse. O app não fala com o banco por esse caminho: fala pelo PostgREST com o papel
`authenticated`, onde a RLS vale.

Evidência de que foi sempre assim: no teste anterior a contagem "depois" apareceu **5** e agora "antes"
aparece **6** — o DELETE que o editor executou de fato removeu uma linha (e o `ROLLBACK` desfez).

## Prova definitiva — só leitura, um resultado, e cobre ALL (falha da view)

A view `verificacao_regras_de_acesso` só acusa `polcmd = 'd'` ou `polcmd = '*'` com `polroles`
contendo 0 (PUBLIC). Uma política `FOR ALL TO authenticated` **escaparia** dessa checagem. A consulta
abaixo fecha essa brecha e prova os dois pontos de uma vez: quem está executando e quantas políticas de
exclusão existem em cada tabela.

```sql
SELECT current_user AS rodando_como,
       c.relname AS tabela,
       pg_get_userbyid(c.relowner) AS dono_da_tabela,
       c.relrowsecurity AS rls_ligada,
       c.relforcerowsecurity AS rls_forcada_no_dono,
       (SELECT count(*) FROM pg_policies p
         WHERE p.schemaname = 'public' AND p.tablename = c.relname
           AND p.cmd IN ('DELETE','ALL')) AS politicas_delete
FROM pg_class c
JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE n.nspname = 'public' AND c.relkind = 'r'
  AND c.relname IN ('oportunidades','clientes','clientes_contatos',
                    'oportunidades_interacoes','oportunidades_documentos',
                    'oportunidades_linha_tempo','oportunidades_etapas',
                    'oportunidades_checklist_itens')
ORDER BY c.relname;
```

Leitura esperada: `rodando_como = postgres`, `dono_da_tabela = postgres`,
`rls_ligada = true`, `rls_forcada_no_dono = false` (explica o `apagadas = 1`) e
`politicas_delete = 0` em todas as linhas (prova de que o app, como `authenticated`, não apaga).

## Lição 4 (a mais importante)

**Teste de RLS não se faz no SQL editor sem trocar de papel.** O editor é `postgres`, dono das tabelas,
e dono ignora RLS. Todo teste de permissão precisa de `SET LOCAL ROLE authenticated` + claims reais, ou
então ser feito pelo próprio app. Testar RLS como dono mede exatamente o contrário do que se quer saber.

## Correção pendente na view (T2.03 ou dívida)

`verificacao_regras_de_acesso` deve passar a considerar `cmd = 'ALL'` **independentemente** de
`polroles` — hoje só pega FOR ALL com PUBLIC. Sem isso, uma política `FOR ALL TO authenticated` fica
invisível para a verificação e reabre o furo em silêncio.
