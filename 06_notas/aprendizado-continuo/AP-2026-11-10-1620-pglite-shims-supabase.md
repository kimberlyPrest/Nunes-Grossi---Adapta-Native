# AP-2026-11-10-1620 — Cadeia completa de migrações no PGlite exige shims do ambiente Supabase

**Origem:** task T4.02 (validação de CA-1-14 a 17 sem Postgres/Docker), 10/11.
**Evidência:** as 24 migrações do repositório aplicaram sem erro em PGlite e a prova 13/13 passou (work/t402-val/pg/prova-t402.mjs).

## Sinal reutilizável

Para aplicar a **cadeia completa** de migrações do projeto em PGlite (não só uma migração isolada),
o ambiente Supabase precisa de shims criados ANTES da primeira migração:

1. `CREATE SCHEMA auth` + `auth.users` com TODAS as colunas que o seed usa (instance_id,
   encrypted_password, raw_app_meta_data, tokens etc.) — senão a migração de seed falha.
2. `auth.uid()` e `auth.jwt()` lendo `current_setting('request.jwt.claims')` — todas as policies usam.
3. `CREATE ROLE authenticated` (+ service_role/anon) — o GRANT delas falha sem existir.
4. **GRANTs que o Supabase aplica sozinho e o PGlite não**: `GRANT SELECT, INSERT, UPDATE, DELETE
   ON ALL TABLES IN SCHEMA public TO authenticated` + EXECUTE em functions — sem isso, `SET ROLE
   authenticated` dá "permission denied for table" em tudo.
5. `storage.buckets` e `storage.objects` (colunas básicas) — a migração do bucket insere nelas.
6. Stubs: `gen_salt`/`crypt` (pgcrypto não existe no PGlite) e `unaccent` como identidade
   (`CREATE EXTENSION` falha; comentar a linha da migração de teste e criar a função stub).
7. Simular usuário: `SET ROLE authenticated` + `set_config('request.jwt.claims',
   json_build_object('sub','<uuid>')::text, false)`; voltar com `RESET ROLE` para contar como dono
   (RLS filtra notificações de outros destinatários).

## Pegadinhas de script (não do sistema)

- `count(*)` volta como **string** no PGlite — comparar com `Number(x) === n`, nunca `=== '12'`.
- Interpolação de UUID opcional: `'${valor}'` com valor null vira string "null" — usar ternário
  para emitir `NULL` literal.
- Fixtures: `profiles.funcao` é NOT NULL; equipes já vêm no seed (usar as existentes); CNPJ de
  teste precisa de DV válido (o CHECK da T3.01 barra — e isso é a prova de que a validação funciona).
- Task já coberta pelo tronco fecha por validação automatizada, sem código novo (T4.02: 13/13).
