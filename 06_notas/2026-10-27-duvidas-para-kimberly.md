# Dúvidas para a Kimberly — 27/10/2026

> **Registradas em `01_projeto/decisoes.md` como D-19 e D-20** (abertas em 27/10, prazo 30/10).
> Este documento é o detalhamento; a resposta oficial entra na tabela de decisões.

Duas perguntas abertas, ambas nascidas de correções já publicadas no sistema. Nenhuma bloqueia o
andamento da Fase 1, mas as duas mudam comportamento que a N&G vai sentir no dia a dia. Preciso da
decisão antes de virar código.

---

## Dúvida 1 (D-19) — Desativar usuário: para quem vão as oportunidades em aberto?

**Quando apareceu:** teste 4 da T2.01 (26/10), levantado pela N&G.

**O que acontece hoje:** ao desativar um usuário, ele perde o acesso (o servidor recusa o login) e
some da lista de responsáveis. Mas o nome dele **continua** como Responsável Comercial em todas as
oportunidades que ele tocava — abertas e concluídas. A ficha da oportunidade segue mostrando um
responsável que não pode mais ser acionado.

**Por que não corrigi por conta própria:** `oportunidades.responsavel_comercial_id` é `NOT NULL` com
`ON DELETE RESTRICT`. Migrar em massa é decisão de negócio, não de implementação: escolher sozinho
para quem iria a carteira de alguém seria inventar regra.

**O que a N&G pediu:** ao desativar, o sistema perguntar para quem migrar as oportunidades
**em aberto e em andamento**; as **concluídas mantêm o nome do desativado** como registro histórico.

**Minha proposta (confirmar ou corrigir):**
- a desativação abre um passo obrigatório: escolher o substituto;
- migram apenas as oportunidades em aberto: **`Em andamento`** e **`Adiada`** — mais o estado
  intermediário **`Ganha — em finalização`**, que ainda não é resultado fechado;
- permanecem intactas: **`Ganha`**, **`Perdida`**, **`Descartada`**, **`Concluída`** e **`Arquivada`**;
- tudo o que migrar fica registrado na linha do tempo da oportunidade (RN-07) — quem trocou, quando e
  por quê;
- se o substituto for definido por equipe/fila, o sistema pode sugerir em vez de exigir.

(o sistema tem hoje exatamente estes oito estados: `Em andamento`, `Adiada`, `Ganha — em
finalização`, `Ganha`, `Perdida`, `Descartada`, `Concluída`, `Arquivada`)

**Perguntas objetivas:**
1. Confirma a proposta acima, ou `Ganha — em finalização` deve manter o nome do desativado?
2. A migração vale também para oportunidades onde ele é responsável **de etapa**, ou só para
   responsável comercial?
3. Existe caso de desativação sem substituto (desligamento de área inteira, por exemplo)? Se sim, o
   que fazer: bloquear a desativação até haver substituto, ou permitir e deixar as oportunidades
   órfãs com aviso?

**Onde entra:** requisito novo, fora do escopo original da SPEC-1-002. Candidata natural: emenda da
SPEC-1-002, executada junto da T2.04 (que já mexe em telas por perfil) ou numa T2.05.

---

## Dúvida 2 (D-20) — Contato do cliente: pode ser excluído?

**Quando apareceu:** efeito colateral da T2.02 (27/10), ao bloquear exclusões em todas as tabelas.

**O que acontece hoje:** com o DELETE bloqueado (RN-06), a ficha do cliente **perdeu** o botão
"Remover contato" — retirado da tela porque a ação sempre falharia. E a tabela `clientes_contatos`
**não tem** campo de situação: só nome, cargo/área, telefone, e-mail e "principal". Resultado: um
contato cadastrado com telefone errado pode ser editado, mas **nunca sai da lista**.

Na prática, quem cadastra o contato de um funcionário que saiu da empresa fica com ele para sempre.

**Minha proposta (confirmar ou corrigir):**
- **Opção A** — criar "inativar contato": nova coluna de situação, o contato sai das listas e do
  seletor de contato principal, continua no histórico. É o mesmo padrão de cliente e usuário.
- **Opção B** — aceitar que contato não se remove nem se inativa: apenas edição. Documentar como
  regra consciente, e a tela passa a deixar claro que o contato permanece no histórico.

**Minha recomendação:** Opção A. A RN-06 fala em "não excluir, arquivar" — hoje o contato é a única
entidade que não tem como ser arquivada, o que é uma exceção silenciosa à própria regra. Além disso,
"contato principal" precisa apontar para alguém ativo; sem inativação, um erro no cadastro contamina
a ficha do cliente para sempre.

**Perguntas objetivas:**
1. Opção A ou B?
2. Se A: o contato inativado pode voltar a ser principal, ou é preciso escolher outro antes?
3. Contato inativado deve continuar aparecendo no histórico de interações antigas, com o nome dele?

**Onde entra:** mudança de esquema (coluna nova + política), exige emenda da SPEC-1-003 (ficha do
cliente) ou task própria. Enquanto não decidido, o sistema está consistente com a regra atual — só
não oferece saída para o caso de erro de cadastro.

---

## Resumo de uma linha

| # | Dúvida | Risco se ficar sem resposta | Onde entra |
|---|---|---|---|
| D-19 | Para quem migram as oportunidades em aberto ao desativar um usuário | Oportunidade fica com responsável inativo e ninguém a assume | Emenda da SPEC-1-002 (T2.04 ou T2.05) |
| D-20 | Contato do cliente pode ser inativado? | Erro de cadastro não tem correção; contato principal pode ficar inativo | Emenda da SPEC-1-003 ou task própria |
