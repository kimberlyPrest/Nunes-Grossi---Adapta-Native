# Debug — T2.02: view de verificação acusa "DELETE ativo" em oportunidades_documentos

**Data:** 26/10/2026 · **Task:** T2.02 · **SPEC:** SPEC-1-002 · **Relatado por:** Pedro (print da consulta, 19:41)

## Sintoma

`SELECT * FROM public.verificacao_regras_de_acesso` retorna 1 linha:
`oportunidades_documentos | DELETE ativo`.

## Diagnóstico

- **Causa:** a migração da T2.02 não tocou a política de DELETE de `oportunidades_documentos`
  (criada na T1.02 como "admin ou autor pode excluir") — o recorte da T2.02 focou nas tabelas onde
  a SPEC falava explicitamente em exclusão (clientes, contatos, interações). A checklist da SPEC,
  porém, manda bloquear exclusão em "clientes, oportunidades **e tabelas dependentes**" — documentos
  é tabela dependente, e a view (corretamente) acusa.
- **Uso real no app:** conferido — **nenhum código do frontend chama `.delete()` em
  `oportunidades_documentos`** (anexar sempre adiciona; a T1.02 tornou o anexo aditivo). O único
  `.delete()` do app é em `clientes_contatos` (`deleteContato`), que agora falha no banco por
  desenho (DELETE zero) — e a tela "Remover contato" da ficha do cliente vai passar a receber
  recusa do banco (caminho de erro obrigatório, mensagem em português — já tratada no serviço).
- **Decisão de correção (dentro da SPEC):** remover a política de DELETE de
  `oportunidades_documentos` (DELETE zero, como nas demais dependentes). Nada no app quebra: não
  existe chamada de exclusão de documento.

## Correção

Migração `20261026230000_t202_fix_delete_documentos.sql`: `DROP POLICY` da política de DELETE de
`oportunidades_documentos`. Reversível (recriar a política anterior).

## Verificação automática

Após aplicar: a view `verificacao_regras_de_acesso` deve voltar **vazia** (0 linhas).

## Gate

`em_correcao` → nova migração publicada → volta a `aguardando_teste_humano` (repetir a consulta).

---

## Debug #2 (27/10) — arquivamento de cliente falha em silêncio para colaborador

**Sintoma:** logado como Colaborador Seguros, o botão "Arquivar" aparece na ficha do cliente; ao
confirmar, o modal fecha e **nada acontece, sem mensagem de erro**.

**Causa raiz (dois defeitos somados):**
1. **Frontend otimista:** `toggleClienteSituacao` fazia `update(...)` sem `.select()` e considerava
   sucesso quando `error` era null. A RLS bloqueia o UPDATE devolvendo **zero linhas sem erro** —
   então o app "confirmava" uma operação que não aconteceu. Esse é o padrão silencioso do Supabase.
2. **Regra de negócio na camada errada:** a política de UPDATE de `clientes` (migração 20261026220000)
   permite o responsável comercial alterar a linha (para editar dados) — e o colaborador de teste
   **é** o responsável comercial do cliente. Resultado: o botão aparecia, o banco recusava a mudança
   de situação, e o erro era engolido pelo item 1.

**Correção aplicada (commit 04b1ee8):**
- `toggleClienteSituacao` passou a usar `.select('id')` e a tratar `data.length === 0` como falha,
  com mensagem em português explicando que só admin/gestor arquivam.
- Botão "Arquivar/Reativar" condicionado a `isAdmin || isGestor` na ficha do cliente (mesmo padrão já
  usado na ficha da oportunidade).
- Migração `20261027132000_t202_fix_arquivar_cliente.sql`: trigger `protege_situacao_cliente` recusa
  mudança de `situacao` em clientes por quem não é Administrador/Gestor. Edição de dados cadastrais
  pelo responsável continua liberada. Duas camadas (tela + banco), como a SPEC exige.

**Observação sobre o arquivamento da oportunidade:** já estava correto — a função
`arquivar_oportunidade` exige explicitamente Administrador/Gestor e o botão só aparece para eles
(`podeArquivar` na ficha). Confirmado no teste 5 do cliente.

**O que restou verificar:** repetir o passo 5 (colaborador não deve ver o botão) e o passo 4 (SQL).
