# Dúvida para a Kimberly — histórico: trigger no banco ou service?

**Aberto em:** 10/11/2026 · **Task travada:** T5.04 (histórico automático, linha do tempo e interações)
**Decidido por Pedro:** aguardar a resposta antes de autorizar a implementação.

## A pergunta, em uma linha

O histórico da oportunidade deve ser gravado **pelo banco** (trigger), como a regra 10 da SPEC-1-005
descreve, ou pode continuar sendo gravado **pelo service da aplicação**, como está hoje?

## Por que isso importa agora

A regra 10 da SPEC-1-005 diz:

> **Histórico:** gravado pelo banco em toda mudança de etapa, responsável, situação, valor, item,
> documento e interação, com usuário e data/hora.

O que encontramos ao mapear o código:

| Evento | Onde é gravado hoje | Cumpre a regra 10? |
|---|---|---|
| Criação da oportunidade | função no banco (`abrir_oportunidade`) | sim |
| Etapa concluída / voltar etapa | função no banco | sim |
| Troca de responsável | função no banco | sim |
| Ganho / perda / arquivamento | função no banco | sim |
| Documento anexado | **service** (app) | parcialmente |
| Interação registrada | **service** (app) | parcialmente |
| Item de checklist concluído | **service** (app) | parcialmente |
| Item de checklist **desmarcado** | **nada** | não |
| Item com **valor editado** depois de concluído | **nada** | não |
| **Valor da oportunidade** | **nada** | não |
| Nº do Quiver | **service** (app) | parcialmente |

"Parcialmente" porque o evento só é gravado se a ação passar pelo service do aplicativo. Uma chamada
direta ao Supabase (ou uma correção feita pelo painel) não deixa rastro.

## Opções

**A — Trigger no banco (fiel à regra 10)**
Triggers em `oportunidades`, `oportunidades_etapas`, `oportunidades_checklist_itens`,
`oportunidades_documentos` e `oportunidades_interacoes`. Grava sempre, com `auth.uid()`.
Exige remover as gravações equivalentes do service, senão cada ação gera **dois** eventos.
Custo: uma migração por tabela + ajuste dos services + regressão completa do histórico.

**B — Manter no service (situação atual)**
Menos mudança e nenhum risco de evento duplicado. Em troca, a regra 10 fica parcialmente não
cumprida pela letra, e os três furos (item desmarcado, valor de item editado, valor da oportunidade)
precisam ser tapados no próprio service.

**C — Híbrido (recomendado pela consultoria, se a Kimberly concordar)**
Trigger apenas onde o service não cobre — valor da oportunidade e item desmarcado/editado — e o
service continua com o que já funciona. Menor risco de duplicidade, fecha os furos reais e não mexe
no que está provado.

## O que a consultoria recomenda

**Opção C**, se o objetivo for fechar os furos sem reescrever um histórico que já funciona e já foi
testado em cinco tasks. **Opção A**, se a leitura da regra 10 for literal e auditoria futura por
chamada direta for um requisito de negócio. A opção B sozinha deixa a regra 10 cumprida "quase" —
e é justamente a palavra que costuma voltar como problema em auditoria.

## O que muda na prática para a N&G

Na opção C, a T5.04 fecha em um ciclo curto. Na opção A, é um ciclo maior com regressão estendida,
porque substituir histórico por trigger mexe em tudo o que hoje registra evento.
