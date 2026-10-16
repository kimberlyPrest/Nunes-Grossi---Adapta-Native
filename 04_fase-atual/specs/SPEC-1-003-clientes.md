# SPEC-1-003 — Cadastro de clientes confiável

**Fase:** 1 — Primeira jornada viva
**Status:** planejada
**Dono:** Maestro (implementação) · Louise (testes)
**Origem:** `04_fase-atual/fase.md` (telas Clientes e Cliente); `01_projeto/regras-e-permissoes.md` (RN-04, RN-06)
**Degrau da solução:** reuso do que o tronco gerou (Prompt 3), com construção mínima de validação no banco

## Resultado observável

A N&G cadastra clientes PF e PJ **sem duplicar e sem documento inválido**, com contatos, origem e quem
indicou, e a ficha do cliente mostra todas as oportunidades dele. É a base limpa que vai receber a
carteira do Quiver na Fase 4.

## Limites e dependências

- **Inclui:** validação dos dígitos de CPF e CNPJ (na tela e no banco); unicidade pelo documento
  **sem máscara** (assim, “12.345.678/0001-90” e “12345678000190” são o mesmo cliente); aviso de cliente
  existente com link; contatos com um único principal; origem e “indicado por”; bloco de oportunidades
  na ficha (abertas, ganhas, perdidas); arquivar e desarquivar; estados vazio, carregando e erro.
- **Fora de escopo:** importação do Quiver (Fase 4); consulta automática à Receita Federal (evolução);
  lista de clientes com dados faltando (Fase 4).
- **Entradas e pré-condições:** SPEC-1-001 e SPEC-1-002 aceitas.
- **Saídas/artefatos:** tela e banco ajustados; seção “Clientes” atualizada no mapa do tronco.
- **Dependências e responsáveis:** Maestro; Louise testa.
- **Risco e plano B:** clientes de teste criados durante o tronco com documentos em formatos misturados.
  **Plano B:** antes de criar a regra de unicidade, a T3.01 normaliza os documentos existentes e lista
  duplicados para a Louise decidir qual manter (arquivando o outro).
- **Rollback ou reversão:** migração reversível; os documentos originais ficam guardados.

## Fluxo e regras

1. Ao digitar o documento, a tela aplica a máscara e valida os dígitos; inválido → “CNPJ inválido” ou “CPF inválido”.
2. Ao sair do campo, o sistema verifica se o documento já existe; se existir → “Este cliente já está cadastrado” + link.
3. O banco recusa documento inválido ou repetido, mesmo que a tela falhe.
4. Marcar um contato como principal desmarca o anterior.
5. Arquivar pede confirmação; o cliente arquivado some da lista padrão (há filtro “mostrar arquivados”) e não aparece na escolha de cliente da Nova oportunidade.
6. Cliente com oportunidade em andamento **não** pode ser arquivado (mensagem explicando).

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | CNPJ válido e novo | Cliente criado; ficha aberta | — |
| Limite | Mesmo CNPJ digitado sem pontuação | Aviso de cliente existente + link | — |
| Falha | CNPJ com dígito errado | Recusado na tela e no banco | Mensagem clara; o campo continua preenchido para corrigir |
| Limite | Arquivar cliente com oportunidade em andamento | Bloqueado com explicação | — |

## Checklist de execução

- [ ] Documentos existentes normalizados; duplicados resolvidos.
- [ ] Validação de dígitos na tela e no banco.
- [ ] Unicidade pelo documento normalizado no banco.
- [ ] Contato principal único.
- [ ] Bloco de oportunidades na ficha, com abas abertas, ganhas e perdidas.
- [ ] Arquivar/desarquivar com as regras acima.
- [ ] Estados vazio, carregando e erro nas duas telas.

## Critérios de aceite

- [ ] **CA-1-10:** CPF ou CNPJ inválido é recusado (teste de aceite 2 da fase).
- [ ] **CA-1-11:** Cadastrar de novo um documento existente, com ou sem pontuação, mostra o aviso e o link para o cadastro existente (teste de aceite 3).
- [ ] **CA-1-12:** A ficha do cliente lista as oportunidades dele, separadas por situação, e só um contato fica como principal.
- [ ] **CA-1-13:** Cliente com oportunidade em andamento não pode ser arquivado; cliente arquivado não aparece na Nova oportunidade.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Tronco aceita CNPJ com dígito errado ou duplicado sem pontuação | Cadastrar 11.111.111/1111-11 e repetir um CNPJ existente sem pontuação | Falha esperada (aceita indevidamente) | Print |
| GREEN | Validação e unicidade ativas | Mesmos cadastros | Os dois são recusados com a mensagem certa | Prints |
| REFACTOR/REGRESSÃO | Cadastro normal e Nova oportunidade continuam funcionando | Cadastrar 1 PF e 1 PJ válidos e abrir uma oportunidade para cada | Passa | Prints |

**Dados/fixtures:** 1 CNPJ válido de teste, 1 CPF válido de teste, 1 CNPJ com dígito errado.
**Caminhos de erro obrigatórios:** documento inválido; duplicado; arquivamento bloqueado; lista vazia.
**Evidência exigida:** prints dos 4 critérios.

## Tasks vinculadas

| ID | Task | Dono | SPEC | Checklist/aceite | Recorte da prova | Status |
|---|---|---|---|---|---|---|
| T3.01 | Normalizar documentos, validar dígitos e garantir a unicidade (tela + banco) | Maestro | SPEC-1-003 | CA-1-10, CA-1-11 | RED → GREEN dos dois cadastros | ☐ |
| T3.02 | Completar a ficha do cliente (oportunidades por situação, contato principal, arquivar com regras, estados de tela) | Maestro | SPEC-1-003 | CA-1-12, CA-1-13 | GREEN + REGRESSÃO | ☐ |

## Emendas

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
