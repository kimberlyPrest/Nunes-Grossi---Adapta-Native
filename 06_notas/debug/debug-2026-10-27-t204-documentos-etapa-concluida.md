# Debug — T2.04 · documentos de etapa concluída sem download

**Data:**  download — T2.04 · documentos de etapa concluída sem download

**Data:** 27/10/2026 · **Task:** T2.04 · **Reportado por:** Pedro (teste humano, item 6 da regressão)

## Sintoma

Depois que uma etapa é concluída, o checklist dela fica somente leitura e lista os documentos
anexados apenas pelo nome. Não existe botão nem link para baixar. O arquivo fica inalcançável
pela interface, inclusive para administrador.

Prints enviados mostram: "CHECKLIST REALIZADO NESTA ETAPA (SOMENTE LEITURA)" com `banner.png` sem ação, e a linha do tempo registrando "Arquivo banner.png anexado com sucesso".

## Causa raiz

O projeto tem **dois** componentes que renderizam itens de checklist:

| Componente | Quando aparece | Download |
|---|---|---|
| `ChecklistEtapaAtual` | etapa atual (editável) | sim — botão "Baixar" com URL assinada |
| `TrilhaEtapas` | etapas concluídas (somente leitura) | **não existia** |

O `TrilhaEtapas` foi escrito com foco em leitura de conteúdo (título, resposta, marcação de
confirmação) e tratou `tipo === 'Documento'` mostrando só `doc.arquivo_nome`. Como o checklist
de uma etapa sai da tela editável assim que a etapa fecha, todo documento de etapa concluída
ficava órfão — a T2.03 provou que o Storage e as políticas funcionam, mas não havia caminho de
UI até eles depois do fechamento.

Não é regressão da T2.04: o defeito é anterior e só apareceu porque a T2.03 passou a exigir link assinado, tornando visível que a tela de leitura não tinha ação alguma.

## Correção

`src/components/oportunidades/TrilhaEtapas.tsx` (commit `0628086`):

- botão "Baixar" por arquivo, tanto na lista `item.documentos` quanto no fallback legado
  (`item.arquivo_nome` + `item.arquivo_url`);
- usa `getDownloadAnexoUrl` — o mesmo link assinado de 5 minutos da etapa atual, portanto sujeito
  às políticas por oportunidade da T2.03;
- estado de carregamento por arquivo (`Loader2`);
- faixa de erro visível quando o link não pode ser gerado (permissão ou falha), em vez de silêncio.

## Verificação automática

- `tsc --noEmit`: 0 erros
- `vite build`: sucesso
- `oxlint src`: 71 warnings = baseline do repositório (nenhum novo)

## Dúvidas e observações levantadas no mesmo teste

- **D-21** (nova, em `01_projeto/decisoes.md`): ao arquivar um cliente, o que acontece com as
  oportunidades dele?
- **Item 5 (gestor) — não é defeito:** a SPEC-1-002 define "Gestor: as das suas equipes". A
  oportunidade da usuária admin apareceu porque ela pertence à mesma equipe do gestor de teste.
  Comportamento conforme a SPEC.
