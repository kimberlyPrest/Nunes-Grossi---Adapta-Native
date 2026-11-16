# Roteiro da leva T7 — Ensaio real e aceite da Fase 1

**Projeto:** Central do Fluxo Comercial N&G · **Fase 1 — Primeira jornada viva**
**Emitido em:** 10/11/2026 · **Revisado em:** 16/11/2026 (trava 4 resolvida)
**Sistema em produção:** v0.0.60 (`index-YqC7kbNV.js`)
**Endereço:** https://central-do-fluxo-comercial-ng-bf9f0.goskip.app
**Base:** SPEC-1-007 (`04_fase-atual/specs/SPEC-1-007-ensaio-real-e-aceite.md`) e `04_fase-atual/fase.md`
**Dono da condução:** Louise · **Ensaio J6:** Isabelle + pós-venda Saúde · **Ensaio J3:** Sheyla · **Aceite:** Viviane

> Esta leva é feita por **pessoas**, não pelo Maestro. Nada aqui é implementação nova: o que surgir de ajuste vira anotação para a Fase 2.

> **O que mudou nesta revisão (16/11):** a trava 4 caiu — a NG-2026-000008 foi concluída de ponta a ponta em 16/11 e virou evidência da CA-1-29 na parte mecânica da J6. Restam **3 travas**. Os números do painel também foram atualizados.

---

## 1. Onde o sistema está hoje (conferido no banco de produção em 16/11)

| Item | Estado real |
|---|---|
| Tasks de implementação | 28 de 35 concluídas (80%) — todas as de código fechadas |
| Contas cadastradas | 6, **todas de teste**: Louise Faraguti (Admin), viviane (Admin), eva.pereira (Gestor), "colaborador saude", "colaborador seguros", "Pós Venda" |
| Clientes | 3: NUNES & GROSSI (9 oportunidades), EMPRESA TESTE COM LETRAS (0), ILLUME MARKETING (arquivado) |
| Oportunidades | 9, **todas no CNPJ da própria corretora** — são de teste |
| Situação delas | **5 Concluída · 2 Perdida · 2 Em andamento** |
| Fluxos ativos | Genérico (5 etapas) · Seguro Empresarial — Porto Seguro (7) · Saúde PME 3 a 29 vidas (12) |
| Painel hoje (out/2026) | 5 ganhas · R$ 10.572,50 · 2 perdidas · R$ 5.000,00 · **2 em andamento · R$ 3.000,00** |
| Painel hoje (nov/2026) | 0 ganhas · 0 perdidas · **2 em andamento · R$ 3.000,00** |

---

## 2. Travas antes de começar

Resolva nesta ordem. Se alguma não estiver resolvida, o ensaio mede a coisa errada.

| # | Trava | Por que trava | Quem resolve |
|---|---|---|---|
| 1 | **T4.00 — revisão de J6 e J3** está aberta desde 29/10 | A SPEC-1-007 exige as SPECs 1-001 a 1-006 aceitas. O ensaio roda sobre os fluxos como estão hoje; se a revisão mudar etapa, o ensaio é refeito | Isabelle (J6) e Sheyla (J3) |
| 2 | **Isabelle, Sheyla e a pessoa do pós-venda Saúde não têm conta** | Sem usuário real não existe "ensaio com pessoas reais" (CA-1-29) | Viviane define perfil/função/equipe; Louise cadastra (T7.01) |
| 3 | **D-18 (Supabase Pro, ~US$ 25/mês) não decidida** | É item de aceite (CA-1-30) | Viviane |

**4. NG-2026-000008 parada na etapa 12 — RESOLVIDA em 16/11.** A etapa 12 foi concluída de verdade pela conta "Pós Venda": a oportunidade saiu de *Ganha — em finalização* para **Concluída**, com as 12 etapas fechadas e a linha do tempo registrando quem concluiu e quando. Isso já vale como evidência da **parte mecânica da J6** (etapas, ganho na 11, passagem para o pós-venda, fim na 12). O ensaio com pessoas reais (item 2) continua necessário para a CA-1-29, mas o caminho já está provado ponta a ponta.

**Perfil/função a definir (não invente — é regra de negócio):** Isabelle na Saúde e Sheyla em Seguros entram como *Colaborador+Comercial* ou *Gestor+Comercial*? A diferença é grande: Gestor vê as oportunidades da equipe inteira e pode arquivar; Colaborador vê só as suas, as que participa e a fila. A pessoa do pós-venda Saúde é *Colaborador + Pós-venda* em *Saúde e Odonto* (é o que a fila da etapa 12 exige — a conta de teste "Pós Venda" já está exatamente assim).

---

## 3. T7.01 — Cadastrar os usuários reais e arquivar os testes
**Quem:** Louise · **Tempo:** ~40 min · **Onde:** tela **Usuários** e ficha de cada oportunidade

### 3.1 Cadastrar as pessoas
1. Entre como Louise Faraguti. Menu lateral → **Usuários**.
   **Esperado:** a lista com as 6 contas e o botão **Convidar usuário** no topo.
2. **Convidar usuário** → nome completo, e-mail corporativo, telefone, **perfil**, **função**, equipe(s) → enviar.
   **Esperado:** "Convite enviado com sucesso!" e a pessoa aparece na lista com etiqueta **Pendente**.
3. Repita para Isabelle, Sheyla e a pessoa do pós-venda.
4. Cada convidado abre o e-mail, clica no link e define a senha em **/definir-senha**.
   **Esperado:** entra no sistema e cai em **Minha fila**.
   **Se não chegar:** na lista, botão **Reenviar** ao lado do nome.

### 3.2 Arquivar as 9 oportunidades de teste
5. Menu **Oportunidades** → filtro de situação **Todas as situações**.
   **Esperado:** as 9 (NG-2026-000001 a 000009), todas do cliente NUNES & GROSSI CORRETORA DE SEGUROS LTDA.
6. Abra cada uma → botão **Arquivar** (canto superior direito da ficha) → escreva o motivo ("oportunidade de teste da Fase 1") → confirmar.
   **Esperado:** a etiqueta de situação vira **Arquivada**; nada é apagado; a linha do tempo ganha "Oportunidade Arquivada".
   **Sobre a NG-2026-000008:** ela foi **concluída em 16/11** e serve de evidência da J6 — **não arquive** antes de tirar o print da ficha concluída e da linha do tempo. Depois do print, pode arquivar junto com as outras.

### 3.3 Provar que saíram da operação (regressão do T7.01)
7. Entre com **colaborador saude** → **Minha fila**.
   **Esperado:** as duas seções vazias, com a mensagem amigável (nada de tela em branco).
8. Entre com **colaborador seguros** → **Minha fila** → mesma coisa.
9. Abra o **Painel** (mês 11/2026).
   **Esperado:** "Em andamento" com 0.
10. Volte à **lista de oportunidades** com filtro **Arquivada**.
    **Esperado:** as 9 aparecem lá — arquivar não apaga.

**Evidência para a tabela:** print do Painel em 0 (passo 9) + print da lista filtrada em Arquivada (passo 10).

**Guarde antes de arquivar (evidência da CA-1-29 pela NG-2026-000008):** print da ficha concluída (selo **Concluída**, as 12 etapas marcadas) e da linha do tempo com "Etapa concluída: Boas-vindas e implantação — Concluída por Pós Venda" e "Fluxo finalizado — Oportunidade Concluída".

**Consulta de conferência (opcional, no SQL Editor do Supabase — cole UMA vez):**

```sql
select 'ativas' as item, count(*)::text as valor
  from oportunidades where situacao <> 'Arquivada'
union all
select 'arquivadas', count(*)::text
  from oportunidades where situacao = 'Arquivada'
union all
select 'usuarios ativos', count(*)::text
  from profiles where situacao = 'ativo';
```

---

## 4. T7.02 — Ensaio J6 (Saúde PME 3 a 29 vidas), com passagem para o pós-venda
**Quem:** Isabelle (comercial) + pessoa do pós-venda Saúde · **Tempo:** ~1h · **Prova:** CA-1-29 (J6)

> Use uma oportunidade **de verdade** (ou um caso real recente, com dado mascarado). Plano B da SPEC: o caso da Hauric, do vídeo.

1. Isabelle entra → botão **Nova oportunidade** (topo; no celular é o botão flutuante).
   Preencha: cliente (existente ou **Novo cliente**), produto **Saúde PME 3 a 29 vidas**, operadora **SulAmérica**, responsável comercial = Isabelle, **origem**, valor estimado, data prevista (dd/mm/aaaa).
   **Esperado:** a ficha abre com **12 etapas** na trilha e a etapa 1 **Lead** como atual.
   **Se aparecer aviso amarelo de "fluxo genérico":** pare — o produto/operadora não casaram com o modelo. É defeito, anote o protocolo.
2. Etapas 1 a 5 (Lead, Qualificação, Cotação multioperadora, Comparativo enviado, Negociação): preencha os itens e clique **Concluir etapa** em cada uma.
   **Esperado:** a trilha avança uma por vez; a etapa concluída mostra quem concluiu; a próxima vira atual.
   **Truque de prova (teste 6):** na etapa 2, tente concluir antes de preencher os obrigatórios → o sistema lista o que falta, com os nomes dos itens.
3. Etapa 6 **Documentação para implantação** — a etapa crítica:
   - Clique **Concluir etapa** sem anexar nada.
     **Esperado:** recusa nomeando os 3 obrigatórios: *Cartão CNPJ, Contrato social, Documentos dos beneficiários*.
   - Em cada um dos 3, botão **Anexar arquivo** → escolha o arquivo.
   - **Concluir etapa** de novo.
     **Esperado:** agora avança.
4. Etapas 7 a 10 (Digitação no portal, Assinatura e críticas, Primeiro boleto, Cancelamento do plano anterior).
5. Etapa 11 **Fechamento**:
   - Marque **Registro feito no Quiver** e preencha **Mensalidade fechada**.
   - **Concluir etapa**.
     **Esperado:** abre sozinho o modal de **confirmação de ganho** — e ele **exige** seguradora, valor e nº da proposta/apólice. Preencha e confirme.
     **Esperado:** situação vira **Ganha — em finalização**, a ficha mostra a **data do ganho**, e a etapa 12 vai para a fila.
6. Na mesma hora, a pessoa do **pós-venda Saúde**:
   - Olha o **sino** no topo.
     **Esperado:** aviso "Nova oportunidade na fila (NG-…)".
   - Abre a oportunidade → **Minha fila**, seção **Fila da minha equipe** → **Assumir etapa** → preenche os 2 itens → **Concluir etapa**.
     **Esperado:** situação **Concluída**; a linha do tempo mostra cada passo com nome e horário.
7. Abra a **linha do tempo** e role do começo ao fim.
   **Esperado:** criação, cada etapa concluída, o ganho confirmado e a passagem para o pós-venda — todos com autor e data/hora.

**Evidência:** protocolo da oportunidade + prints da etapa 6 recusando, do modal de ganho, do sino do pós-venda e da linha do tempo.

---

## 5. T7.03 — Ensaio J3 (Seguro Empresarial — Porto Seguro)
**Quem:** Sheyla · **Tempo:** ~45 min · **Prova:** CA-1-29 (J3)

> A J3 tem **7 etapas**. A etapa 5 (**Registro no Quiver — pendente**) é a etapa de ganho do fluxo: ao concluí-la, a oportunidade vira **Ganha — em finalização** e **continua** para a 6.

1. Sheyla entra → **Nova oportunidade** → cliente real, produto **Seguro Empresarial**, seguradora **Porto Seguro**, responsável = Sheyla, origem, valor, data prevista.
   **Esperado:** ficha com **7 etapas**, etapa 1 **Coleta de dados do risco** atual.
2. Etapa 1 (9 itens, 7 obrigatórios) e etapa 2 **Cotação**.
3. Etapa 3 **Cotação enviada** e etapa 4 **Elaborar proposta**.
4. Etapa 5 **Registro no Quiver — pendente** → itens + **Concluir etapa**.
   **Esperado:** abre o modal de ganho (é a etapa de ganho); confirme → **Ganha — em finalização**, data do ganho na ficha.
5. Etapa 6 **Aguardando apólice** → concluir.
6. Etapa 7 **Apólice emitida** (4 itens, 3 obrigatórios) → concluir.
   **Esperado:** situação **Concluída**.
7. Confira no **Quadro** que o cartão dela esteve na coluna certa ao longo do caminho (isso alimenta o teste 12).

**Evidência:** protocolo + print da etapa 5 virando "Ganha — em finalização" e do fim em "Concluída".

---

## 6. T7.04 — Os 13 testes de aceite, com evidência
**Quem:** Louise · **Onde:** sistema em produção · **Prova:** CA-1-28

> Faça depois dos dois ensaios: o teste 13 depende dos números que vocês acabaram de produzir. **Antes de começar, tire um print do Painel** (é a coluna "antes").

| # | O que fazer | O que deve acontecer |
|---|---|---|
| 1 | Entrar como **colaborador** e procurar **Usuários** e **Configurações** no menu; depois digitar `/usuarios` no endereço | Não aparecem no menu; por endereço direto, tela **"sem acesso"** |
| 2 | **Novo cliente** com CNPJ inválido (ex.: `11.111.111/1111-11`) | Recusa, com mensagem em português |
| 3 | **Novo cliente** com o CNPJ que já existe (`06.062.276/0001-80`) | Avisa que já existe e oferece **"Abrir a ficha do cliente existente"** |
| 4 | **Nova oportunidade** → produto **Saúde PME 3 a 29 vidas** + operadora **SulAmérica** | Nascem as **12 etapas** da J6, com a etapa 1 atual e o responsável comercial |
| 5 | **Nova oportunidade** → produto **Saúde PME 30 a 99 vidas** (não tem fluxo próprio) | Usa o **fluxo genérico** (5 etapas) e a ficha mostra o **aviso amarelo** explicando |
| 6 | Na etapa **Documentação para implantação** (J6), tentar **Concluir etapa** sem o cartão CNPJ | Recusa e nomeia os itens que faltam |
| 7 | Anexar um documento na J6 e entrar como **colaborador seguros** (`louise.corretorang@gmail.com`): abrir a ficha pelo endereço direto e tentar baixar o arquivo | **"sem acesso"** na ficha e o documento não abre |
| 8 | Concluir a **etapa 11** da J6 | Vai para a fila do pós-venda **e** o sino de quem é da fila mostra o aviso |
| 9 | Numa oportunidade em andamento, **Voltar etapa** sem escrever o motivo; depois escreva | Sem motivo não deixa; com motivo, ele aparece na **linha do tempo** |
| 10 | **Marcar como perdida** sem escolher motivo | Botão não libera; se forçar, "Selecione obrigatoriamente um motivo de perda" |
| 11 | Como **colaborador**, abrir uma oportunidade e procurar excluir | Não existe botão de excluir (**nem o Arquivar**, que é só de Admin/Gestor) |
| 12 | Abrir o **Quadro** e comparar com a ficha | O cartão está na coluna da **etapa atual** |
| 13 | Abrir o **Painel** e comparar com os ensaios | Ganhas e perdidas do mês batem em **quantidade e valor**; clicar no número abre a lista exata |

**Evidência por linha:** print da tela + protocolo. Na 13, print do Painel antes e depois.

**Atenção no teste 2:** se usar o CNPJ alfanumérico oficial (`00.000.000/E08G-12`), o nono caractere é **zero**, não a letra **O** — com "EO8G" o sistema recusa, e isso é o comportamento correto.

---

## 7. T7.05 — D-18, demonstração e aceite
**Quem:** Viviane · **Prova:** CA-1-30

1. **D-18:** decidir a contratação do **Supabase Pro** (~US$ 25/mês) para uso em produção. Registrar a decisão com data.
2. **Demonstração** (roteiro de 5 pontos, em `04_fase-atual/fase.md`):
   - Login com a identidade da N&G, cada pessoa com o seu acesso.
   - Cadastro de cliente pelo CNPJ, com contatos e origem.
   - **Nova oportunidade**: cliente + produto + operadora → as 12 etapas da J6 aparecem com o checklist.
   - Documento anexado, item marcado, etapa concluída → a oportunidade **aparece sozinha** na fila do pós-venda.
   - **Quadro** por etapa e **Painel** com em andamento, ganhas e perdidas do mês.
3. **Aceite** registrado no grupo do projeto ou na Jornada de Execução (print ou mensagem arquivada).

---

## 8. Se não bater — o que cada sintoma significa

| Você vê | Significa | O que fazer |
|---|---|---|
| "sem acesso" onde deveria entrar | Conta com perfil/equipe errada | Confira o perfil na tela Usuários; não é defeito do fluxo |
| A ficha abre "Ganha — em finalização" e não sai disso | Falta concluir a etapa 12 (pós-venda) — é o desenho, não um bug | Concluir a etapa 12 (a NG-2026-000008 já passou por isso em 16/11) |
| O Painel mostra número de "em andamento" maior do que você esperava | "Em andamento" **inclui** "Ganha — em finalização" (definição da SPEC-1-006) | Confira somando as duas situações |
| A tela pisca ou volta ao login | Sessão expirada | Recarregue e entre de novo; se repetir, anote a hora |
| Um documento de outra equipe abre | **Defeito grave** (regra do Storage) | Pare o ensaio, guarde o print e o nome do arquivo, avise a Kimberly |
| O sistema recusa uma etapa que parece completa | Falta item obrigatório — a mensagem nomeia | Preencha o que ele listou |
| O ensaio usa um fluxo que a revisão do T4.00 vai mudar | Ordem invertida | Faça o T4.00 antes; senão o ensaio é descartado |

**Regra:** sintoma com todas as travas verdes = defeito real. Guarde **protocolo + print + hora** e mande no grupo. Não tente contornar por dentro do banco.

---

## 9. Sequência recomendada (o que fazer, na ordem)

| Dia | O quê | Quem |
|---|---|---|
| 1 | T4.00 — revisar J6 e J3 nas telas de Configurações → Fluxos | Isabelle, Sheyla |
| 1 | Definir perfil/função de Isabelle, Sheyla e pós-venda · decidir D-18 | Viviane |
| 2 | T7.01 — convidar as pessoas e arquivar as 9 oportunidades de teste (print da NG-2026-000008 antes) | Louise |
| 3 | T7.02 — ensaio J6 | Isabelle + pós-venda |
| 3 | T7.03 — ensaio J3 | Sheyla |
| 4 | T7.04 — os 13 testes com evidência | Louise |
| 5 | T7.05 — demonstração + aceite | Viviane |

Ao fechar o T7.05, a Fase 1 está encerrada: 35 de 35.

---

## 10. O que ainda vai sobrar em aberto (para não virar surpresa no aceite)

- **T4.03** — aplicar a revisão de Isabelle e Sheyla nos modelos J6 e J3 (depende do T4.00).
- **D-19, D-20, D-21** — oportunidades ao desativar usuário, inativar contato, arquivar cliente. Abertas desde 27/10.
- **Emendas de SPEC pendentes** — regra 10 da SPEC-1-005 (histórico, opção C da D-24) e regras 3/10.
- **CA-1-08 e CA-1-24/CA-1-26** — critérios sem evidência formal na tabela, apesar de o comportamento existir. Fecham junto com o T7.04 se você fotografar os itens correspondentes.
- **Espelho GitHub do código** parado em 16/10 — o que está no ar é a v0.0.60 publicada pelo Skip, não o espelho.
- **Autor da conclusão de etapa (achado de 16/11, Fase 2)** — a coluna que guarda "quem concluiu" (`concluida_por_id`) fica vazia; a tela mostra o nome pelo responsável da etapa. Funciona em todos os casos do ensaio, mas erraria o nome se uma etapa de fila já assumida fosse concluída por um colega (a D-23 permite). A linha do tempo registra o autor certo de qualquer forma.
