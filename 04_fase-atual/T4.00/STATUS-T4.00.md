# T4.00 — revisão dos fluxos: enviada, aguardando resposta

**Status:** **enviado** — aguardando resposta de **Isabelle** (fluxo Saúde) e **Sheyla** (fluxo Empresarial)
**Enviado em:** 16/11/2026 às 14:02 · **Prazo proposto:** 20/11/2026 · **Cobrança:** 19/11 se não houver resposta
**Dono da cobrança:** Pedro (owner) · **Dona da condução:** Louise

---

## O que esta task pede

A SPEC-1-004 prevê que Isabelle e Sheyla revisem os modelos J6 e J3 e devolvam as anotações; o que
elas apontarem é aplicado pela equipe (T4.03). É a última pendência antes dos ensaios da leva T7.

## Impedimento encontrado (e como foi resolvido)

A task, como está escrita, manda as duas revisarem em **Configurações → Fluxos**. Essa página é
**exclusiva de Administrador** — confirmado no código (`App.tsx` envolve a rota no `AdminRoute`) e no
bundle de produção (`index-YqC7kbNV.js`, sha256 `8bd25aeb`). Como gestoras/colaboradoras, elas
receberiam a tela "Você não tem acesso".

A própria SPEC-1-004 autoriza a saída: *"Isabelle e Sheyla devolvem a revisão (prints da página
Fluxos com anotações bastam); o Maestro aplica"*. Então a revisão foi levada até elas:

| Caminho | Como funciona |
|---|---|
| **A — tela compartilhada** | Louise (Admin) abre e compartilha; elas ditam |
| **B — print + resposta escrita** *(escolhido)* | Louise tira os prints; cada uma responde por escrito |
| ~~C — promover a Administrador~~ | Descartado: Administrador vê todas as equipes e pode arquivar |

## Como foi enviado

**Por e-mail, em 16/11/2026 às 14:02**, pela conta corporativa, com o texto de `textos-de-envio.md`:

- para a **Isabelle**: o PDF do próprio fluxo (J6) + os prints das telas do modelo no sistema;
- para a **Sheyla**: o PDF do próprio fluxo (J3) + os prints das telas do modelo no sistema.

Cada uma recebeu **só o seu fluxo**. Nenhuma das duas precisa de login ou acesso ao sistema para
revisar.

## Material enviado

| Arquivo | Para quem | O que é |
|---|---|---|
| `orientacoes-Isabelle.md` | Isabelle | Fluxo J6 (Saúde PME 3 a 29 vidas), 12 etapas, 33 itens, 24 obrigatórios, 3 perguntas |
| `orientacoes-Sheyla.md` | Sheyla | Fluxo J3 (Seguro Empresarial — Porto Seguro), 7 etapas, 25 itens, 20 obrigatórios, 2 perguntas |
| `textos-de-envio.md` | — | O texto que acompanha cada envio, no e-mail ou no WhatsApp |

Em PDF (mesmo conteúdo, para anexar) e em planilha (para quem preferir preencher): ficam no Maestro,
em `artifacts/`. **Não foram publicados no repositório** porque o conector do GitHub grava texto e
corromperia os binários — testado em 16/11.

## As perguntas que só elas podem responder

**Isabelle (J6):**
- Etapa 6 — Documentação para implantação: quem faz de verdade, o comercial ou a implantação/pós-venda?
- Etapa 7 — Digitação no portal da operadora: quem digita hoje? É a mesma pessoa da etapa 6?
- Etapa 10 — Cancelamento do plano anterior: a carta sai só depois da implantação e do pagamento?

**Sheyla (J3):**
- Etapa 1 — Coleta de dados do risco: os 7 itens obrigatórios (de 9) são mesmo obrigatórios para cotar?
- Etapa 5 — Registro no Quiver: a etapa deve existir separada, ou o registro faz parte da proposta?

## O que acontece quando responderem

1. As anotações são registradas como recebidas e a T4.00 fecha.
2. A **T4.03** aplica o que for apontado nos modelos, por migração, e publica pelo Skip.
3. Elas conferem de novo na tela antes do ensaio — a **CA-1-18** ("a página Fluxos mostra exatamente o
   que Isabelle e Sheyla aprovaram") só fecha com essa conferência.
4. Só então começam os ensaios da leva T7 (ver `04_fase-atual/roteiro-leva-T7.md`).

## Trava ativa

Sem a resposta das duas, **5 tasks ficam paradas**: T4.03, T7.02, T7.03, T7.04 e T7.05. A T4.00 está
aberta desde 29/10.
