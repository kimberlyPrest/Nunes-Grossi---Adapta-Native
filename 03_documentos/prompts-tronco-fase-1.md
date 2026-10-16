# Fase 1 — Prompts do tronco (enviar direto no Skip)

> **Para quem:** Louise (executora), com apoio da Kimberly.
> **Fase:** 1 — Primeira jornada viva · **Fase atual:** `04_fase-atual/fase.md`.

## O que é o “tronco” e por que começamos por ele

Tronco antes de galhos. Estes 10 prompts criam o **esqueleto inteiro da Fase 1** de uma vez:
telas, navegação e as informações básicas. Quando um prompt é enviado no Skip, ele já cria **ao mesmo
tempo** as telas e as tabelas do banco de dados de que essas telas precisam. Por isso, os prompts
descrevem **o que a tela mostra e quais informações ela guarda**, e o Skip monta o banco sozinho.

Depois do tronco, o Maestro entra com as **SPECs** (pasta `04_fase-atual/specs`): primeiro ele **confere o que o
Skip realmente criou** (telas, tabelas, campos e ligações) e, a partir disso, completa os **galhos**:
segurança, validações, regras do fluxo, painel e os detalhes de cada tela.

## Regras de ouro para enviar os prompts

1. **Um prompt por vez, na ordem.** Cada um se apoia no anterior.
2. **Espere o Skip terminar** antes de mandar o próximo (alguns minutos).
3. **Faça a conferência rápida** que vem depois de cada prompt (2 a 5 minutos). Se algo **essencial**
   faltar, mande **uma mensagem curta de correção** no mesmo chat (“Na tela de clientes faltou o campo
   CNPJ”). Não reescreva o prompt inteiro.
4. **Detalhes pequenos ficam para depois.** Cor de botão, texto de aviso ou ordem de coluna: anote em
   “Observações do tronco”, no fim deste arquivo. O Maestro resolve nas SPECs.
5. **Não crie dados de exemplo** de clientes ou oportunidades. Os únicos dados iniciais são os do
   catálogo e dos fluxos, que os prompts pedem explicitamente.
6. **Confira o banco** depois de cada prompt: clique na nuvem roxa (banco de dados) e veja se as
   tabelas citadas na conferência apareceram.
7. Se o Skip **travar ou quebrar algo**, pare e mande um print no grupo antes de continuar.

---

## Passo 0 — Conectar o banco de dados (antes do Prompt 1)

**Quem:** Louise, com a Kimberly na chamada.

1. Criar o projeto no **Supabase** com a conta da N&G (e-mail corporativo), região **South America
   (São Paulo)**. Nome sugerido: `central-comercial-ng`.
2. No Skip, abrir o projeto da tela de login criado em 05/10 (“login corretora”) → configurações /
   integrações → **conectar o Supabase**, escolhendo o projeto criado.
3. Guardar a senha do banco do Supabase em local seguro (gerenciador de senhas). **Nunca** enviar por
   WhatsApp nem colar em prompt.

**Conferência:** nas configurações do Skip, o Supabase aparece como conectado ao projeto.

> Se, ao abrir as integrações, o Skip estiver usando só o banco nativo dele (nuvem roxa), avise a
> Kimberly antes de enviar o Prompt 1: tudo o que os prompts criam vai para o banco que estiver
> conectado nesse momento.

---

## Prompt 1 — Login, estrutura do sistema e navegação

> Este prompt **inicia um projeto novo do zero** no Skip (Início → Iniciar novo projeto → Prompt
> livre). Não usamos a tela de login criada em 05/10. Antes de enviar, troque `[ENDEREÇO DO SITE]`
> pelo site da corretora.

```
Crie a "Central do Fluxo Comercial N&G", o sistema comercial interno da Nunes & Grossi Corretora de Seguros. Use a logo e as cores do site da corretora: [ENDEREÇO DO SITE].

Tela de login:
- logo da N&G, campos de e-mail e senha e botão Entrar;
- link "Esqueci minha senha", que envia um e-mail para criar uma nova senha;
- não existe cadastro aberto: só entra quem for convidado por um administrador (o convite será criado depois);
- mensagens de erro em português simples (ex.: "E-mail ou senha incorretos").

Depois do login, crie a estrutura do sistema:
- Um menu lateral com: Minha fila, Oportunidades, Quadro, Clientes, Painel e, no fim, Usuários.
- Um topo com o nome da pessoa logada, um sino de notificações (por enquanto vazio) e o botão Sair.
- Cada item do menu abre uma página própria. Por enquanto as páginas podem ficar com o título e uma mensagem de "vazio" amigável, por exemplo: "Nenhuma oportunidade ainda — crie a primeira".
- Um botão de destaque "Nova oportunidade" sempre visível no topo.

Padrões que valem para o sistema inteiro:
- Tudo em português do Brasil.
- Datas no formato dd/mm/aaaa e valores em R$.
- Toda página tem os estados carregando, vazio e erro, com mensagem em português simples, sem termos técnicos.
- Layout que funciona bem no computador e é usável no celular.
- Não crie dados de exemplo.
```

**Conferência rápida:** a tela de login aparece com a logo e as cores da N&G → “Esqueci minha senha”
envia o e-mail → entrar com o login → o menu aparece com os 6 itens → cada item abre uma página
com mensagem de vazio → o botão “Nova oportunidade” aparece no topo.

---

## Prompt 2 — Usuários, perfis, equipes e funções

```
Crie a gestão de usuários do sistema.

Cada usuário tem:
- nome, e-mail, telefone;
- perfil de acesso: Administrador, Gestor ou Colaborador;
- uma ou mais equipes: "Seguros" e/ou "Saúde e Odonto" (crie essas duas equipes já cadastradas);
- função: Comercial, Pós-venda ou Gestão;
- situação: ativo ou inativo.

Página "Usuários" (só Administrador vê e acessa):
- lista com nome, e-mail, perfil, equipes, função e situação, com busca;
- botão para convidar um usuário por e-mail, já escolhendo perfil, equipes e função;
- editar perfil, equipes e função;
- desativar e reativar. Usuário inativo não consegue entrar no sistema.

Regras de acesso a respeitar:
- O item "Usuários" do menu só aparece para Administrador.
- Gestor e Colaborador não podem mudar o próprio perfil.
- O primeiro usuário (o dono desta conta) é Administrador.
```

**Conferência rápida:** abrir Usuários → as equipes “Seguros” e “Saúde e Odonto” existem → convidar
um e-mail de teste como Colaborador → entrar com ele: o item Usuários não aparece no menu.
**No banco:** aparecem tabelas de usuários/perfis e de equipes.

---

## Prompt 3 — Clientes e contatos

```
Crie o cadastro de clientes da corretora.

Um cliente pode ser Pessoa Jurídica ou Pessoa Física.
- Pessoa Jurídica: CNPJ, razão social, nome fantasia, segmento ou atividade, porte (micro, pequena, média, grande), número de vidas (opcional), cidade e UF.
- Pessoa Física: CPF, nome completo, data de nascimento, cidade e UF.
- Para os dois: origem (de onde o cliente veio), indicado por (tipo: cliente, colaborador ou parceiro, mais o nome de quem indicou), responsável comercial (um usuário do sistema), observações e situação (ativo ou arquivado).

O cliente tem vários contatos: nome, cargo ou área (RH, Financeiro, Sócio, Administrativo, Outro), telefone, e-mail e uma marcação de "contato principal" (só um por cliente).

Página "Clientes":
- lista com nome/razão social, CPF/CNPJ, tipo, segmento, responsável e quantidade de oportunidades abertas;
- busca por nome ou documento;
- botão "Novo cliente".

Página da ficha do cliente:
- dados do cliente com botão de editar;
- bloco de contatos (incluir, editar, marcar principal);
- bloco de oportunidades do cliente (por enquanto pode ficar vazio);
- botão "Nova oportunidade para este cliente".

Regras:
- CPF e CNPJ não podem se repetir. Se alguém tentar cadastrar um documento que já existe, mostre "Este cliente já está cadastrado" com um link para a ficha existente.
- Nenhum cliente é apagado; só arquivado.
- Ao digitar o CNPJ, aplique a máscara 00.000.000/0000-00; no CPF, 000.000.000-00.
```

**Conferência rápida:** cadastrar um cliente PJ com um CNPJ real de teste → incluir um contato principal
→ tentar cadastrar o mesmo CNPJ de novo: deve avisar e mostrar o link.
**No banco:** tabelas de clientes e de contatos.

---

## Prompt 4 — Catálogo: seguradoras, produtos, origens e motivos

```
Crie o catálogo que o sistema usa nas oportunidades. Nesta etapa não precisa de telas de edição; crie apenas as informações e os dados iniciais abaixo, e uma página "Configurações > Catálogo", somente para Administrador, que apenas mostra as listas (sem editar).

1) Seguradoras e operadoras: nome, frente (Seguros ou Saúde), canal de cotação (Portal, Multicálculo, E-mail ou WhatsApp) e situação (ativa/inativa). Dados iniciais:
- Porto Seguro (Seguros, Portal)
- Tokio Marine (Seguros, Portal)
- Akad Digital (Seguros, Portal)
- Capemisa (Seguros, Portal)
- Porto Saúde (Saúde, Portal)
- SulAmérica (Saúde, Portal)
- Ana Costa Saúde (Saúde, Multicálculo)
- Santa Saúde (Saúde, Multicálculo)

2) Produtos: nome, frente (Seguros ou Saúde), equipe responsável (Seguros ou Saúde e Odonto) e situação. Dados iniciais:
- Seguros: Auto/Moto, RC Profissional, Seguro Empresarial, RC Eventos, AP Eventos, Vida, Frota, Residencial, D&O, Garantia, Risco Cibernético
- Saúde: Saúde PME 3 a 29 vidas, Saúde PME 30 a 99 vidas, Odonto

3) Origens de lead (dados iniciais): Indicação de cliente, Indicação de colaborador, Parceiro, Redes sociais/LinkedIn, Site, QuiverZap/WhatsApp geral, Carteira (cliente ativo), Outro.

4) Motivos, cada um com o tipo (perda, descarte ou adiamento). Dados iniciais de perda: Preço, Optou por outra corretora, Optou pelo plano do sindicato/categoria, Cliente desistiu, Sem retorno do cliente, Momento inadequado, Não aprovado pela seguradora/operadora, Cobertura não atende, Outro.

Na tela de cadastro de clientes, o campo "origem" deve usar essa lista de origens.
Coloque o item "Configurações" no menu, visível só para Administrador.
```

**Conferência rápida:** Configurações → Catálogo mostra as 4 listas com os dados acima → no cadastro
de cliente, a origem vira uma lista de escolha.
**No banco:** tabelas de seguradoras, produtos, origens e motivos, já com os dados.

---

## Prompt 5 — Estrutura dos fluxos + fluxo genérico + fluxo Empresarial (Porto)

```
Agora crie o coração do sistema: os MODELOS DE FLUXO. Um modelo de fluxo diz quais etapas uma oportunidade percorre e o que precisa ser feito em cada etapa.

Um modelo de fluxo tem: nome, produto, seguradora (opcional — vazio quando vale para qualquer seguradora daquele produto), indicação se é o fluxo genérico, versão (começa em 1) e situação (ativo/inativo).

Cada modelo tem ETAPAS em ordem. Cada etapa tem:
- nome e descrição;
- tipo: "Trabalho interno", "Aguardando cliente" ou "Aguardando seguradora";
- responsável padrão, que pode ser: "Responsável comercial da oportunidade" ou "Fila de uma equipe + função" (por exemplo, fila Saúde e Odonto / Pós-venda);
- prazo sugerido em dias (só informativo por enquanto);
- marcação "ao concluir esta etapa, a oportunidade é ganha" (etapa de ganho).

Cada etapa tem ITENS DE CHECKLIST em ordem. Cada item tem: título, tipo ("Documento" = anexar arquivo; "Informação" = campo a preencher; "Confirmação" = marcar como feito), formato do campo quando for Informação (texto, número, valor em R$, data, sim/não ou lista de opções) e se é obrigatório.

Crie uma página "Configurações > Fluxos", só para Administrador e só para consulta (sem editar), que lista os modelos e, ao abrir um, mostra as etapas e os itens.

Cadastre estes dois modelos:

A) FLUXO GENÉRICO (vale para qualquer produto que não tenha fluxo próprio). Todas as etapas com responsável = responsável comercial da oportunidade:
1. Lead (Trabalho interno) — Origem do contato (Informação, texto, obrigatório)
2. Qualificação (Aguardando cliente) — Necessidade do cliente (Informação, texto, obrigatório)
3. Cotação (Trabalho interno) — Cotação anexada (Documento, obrigatório)
4. Proposta enviada (Aguardando cliente) — Proposta enviada ao cliente (Confirmação, obrigatório)
5. Negociação e fechamento (Aguardando cliente) — Cliente aprovou (Confirmação, obrigatório) — ETAPA DE GANHO

B) SEGURO EMPRESARIAL — PORTO SEGURO (produto Seguro Empresarial, seguradora Porto Seguro). Responsável de todas as etapas = responsável comercial da oportunidade:
1. Coleta de dados do risco (Aguardando cliente): Tipo de pessoa e CPF/CNPJ (Informação, texto, obrigatório); Motivo, ex. profissional liberal ou locatário (Informação, texto, obrigatório); Endereço do risco (Informação, texto, obrigatório); Atividade exercida no local (Informação, texto, obrigatório); Valores desejados por cobertura — incêndio, danos elétricos, vidros, aluguel, RC, RC empregador, danos morais (Informação, texto, obrigatório); Imóvel alugado? (Informação, sim/não, obrigatório); Aluguel mensal (Informação, valor, opcional); Dados do proprietário para cláusula beneficiária (Informação, texto, opcional); Sinistros nos últimos 12 meses (Informação, sim/não, obrigatório)
2. Cotação (Trabalho interno): PDF da cotação (Documento, obrigatório); Conferido: Danos Morais não passa de 100% da RC (Confirmação, obrigatório); Indenização a valor de novo definida (Confirmação, opcional); Franquias revisadas (Confirmação, opcional)
3. Cotação enviada (Aguardando cliente): Cotação enviada ao cliente (Confirmação, obrigatório); Validade da cotação (Informação, data, obrigatório); Cliente aprovou a cotação (Confirmação, obrigatório)
4. Elaborar proposta (Trabalho interno): E-mail, estado civil, profissão, PPE e endereço de cobrança (Informação, texto, obrigatório); Proposta finalizada na seguradora (Confirmação, obrigatório)
5. Registro no Quiver — pendente (Trabalho interno): Nº da proposta da companhia (Informação, texto, obrigatório); Prêmio, parcelas e comissão (Informação, texto, obrigatório) — ETAPA DE GANHO
6. Aguardando apólice (Aguardando seguradora, prazo 1 dia): Apólice recebida da seguradora (Confirmação, obrigatório)
7. Apólice emitida (Trabalho interno): Nº e data de emissão lançados no Quiver (Confirmação, obrigatório); Apólice (Documento, obrigatório); Boleto (Documento, opcional); Apólice e boleto enviados ao cliente (Confirmação, obrigatório)
```

**Conferência rápida:** Configurações → Fluxos mostra os 2 modelos → abrir o Empresarial: 7 etapas,
com os itens e os obrigatórios corretos.
**No banco:** tabelas de modelos de fluxo, etapas e itens.

---

## Prompt 6 — Fluxo Saúde PME 3 a 29 vidas (SulAmérica / Porto Saúde)

```
Cadastre mais um modelo de fluxo, usando a mesma estrutura de modelos, etapas e itens que já existe:

SAÚDE PME 3 A 29 VIDAS — produto "Saúde PME 3 a 29 vidas", sem seguradora fixa (vale para SulAmérica, Porto Saúde e outras operadoras desse produto).

Responsável: etapas 1 a 9 e 11 = responsável comercial da oportunidade; etapas 10 e 12 = fila Saúde e Odonto / Pós-venda.

1. Lead (Trabalho interno): Origem (Informação, texto, obrigatório); Parceiro ou indicador (Informação, texto, opcional)
2. Qualificação (Aguardando cliente): CNPJ (Informação, texto, obrigatório); Nº de titulares e dependentes (Informação, número, obrigatório); Idades dos beneficiários (Informação, texto, obrigatório); Cidade (Informação, texto, obrigatório); Plano atual e tempo de permanência (Informação, texto, obrigatório); Hospital ou rede de preferência (Informação, texto, opcional); Com ou sem coparticipação (Informação, lista: Com / Sem / Indiferente, opcional)
3. Cotação multioperadora (Trabalho interno): Cotações anexadas (Documento, obrigatório); Operadoras cotadas (Informação, texto, obrigatório)
4. Comparativo enviado (Aguardando cliente): Comparativo enviado ao cliente (Confirmação, obrigatório); Data do próximo contato (Informação, data, obrigatório)
5. Negociação (Aguardando cliente): Dúvidas do cliente tratadas (Confirmação, opcional); Operadora e plano escolhidos (Informação, texto, obrigatório)
6. Documentação para implantação (Aguardando cliente): Cartão CNPJ (Documento, obrigatório); Contrato social (Documento, obrigatório); Documentos dos beneficiários — CPF, RG, nome da mãe, nascimento (Documento, obrigatório); Comprovante de vínculo dos dependentes (Documento, opcional); Carteirinha ou comprovante do plano anterior (Documento, opcional); Carta de não adesão de sócio + RG + comprovante de outro plano (Documento, opcional)
7. Digitação no portal da operadora (Trabalho interno): Proposta digitada e enviada para análise (Confirmação, obrigatório)
8. Assinatura e críticas (Aguardando cliente): Assinatura digital do cliente (Confirmação, obrigatório); Críticas da operadora resolvidas (Confirmação, obrigatório)
9. Primeiro boleto (Aguardando cliente): Boleto enviado (Confirmação, obrigatório); Pagamento confirmado (Confirmação, obrigatório)
10. Cancelamento do plano anterior (Aguardando cliente): Orientação ou modelo de carta de cancelamento enviado, somente depois da implantação e do pagamento do novo plano (Confirmação, obrigatório)
11. Fechamento (Trabalho interno): Registro feito no Quiver (Confirmação, obrigatório); Mensalidade fechada (Informação, valor, obrigatório) — ETAPA DE GANHO
12. Boas-vindas e implantação (Trabalho interno): Materiais de acesso enviados (Confirmação, obrigatório); Contato do RH registrado (Informação, texto, obrigatório)

Importante: não guarde informações médicas. O sistema só registra se os documentos foram entregues.
```

**Conferência rápida:** Configurações → Fluxos mostra 3 modelos → abrir o Saúde PME: 12 etapas, e as
etapas 10 e 12 com responsável = fila Saúde e Odonto / Pós-venda.

---

## Prompt 7 — Nova oportunidade (o fluxo aparece sozinho)

```
Crie a abertura de OPORTUNIDADES.

Ao clicar em "Nova oportunidade" (no topo ou na ficha do cliente), abra um formulário com:
- cliente (busca pelos cadastrados, com a opção "cadastrar cliente rápido" com tipo, documento e nome);
- produto (da lista de produtos ativos);
- seguradora/operadora (lista filtrada pela frente do produto: Seguros ou Saúde);
- responsável comercial (usuário; o padrão é quem está criando);
- origem (lista de origens);
- valor estimado em R$ (prêmio ou mensalidade estimada);
- data prevista de fechamento;
- observações.

Ao salvar:
1. O sistema escolhe o modelo de fluxo nesta ordem: (a) modelo ativo do mesmo produto e da mesma seguradora; (b) modelo ativo do mesmo produto sem seguradora fixa; (c) fluxo genérico.
2. A oportunidade recebe um número de protocolo no formato NG-ANO-NÚMERO sequencial (exemplo: NG-2026-000001).
3. A oportunidade guarda SUA PRÓPRIA CÓPIA das etapas e dos itens do checklist do modelo, com a versão do modelo usada. Assim, se o modelo mudar no futuro, as oportunidades já abertas não mudam.
4. A primeira etapa fica como etapa atual, com o responsável definido pela regra da etapa.
5. A oportunidade nasce com situação "Em andamento" e a equipe do produto.
6. Abre a ficha da oportunidade recém-criada.

Se o fluxo genérico foi usado, mostre o aviso: "Este produto ainda não tem um fluxo próprio; usamos o fluxo padrão."
```

**Conferência rápida:** criar uma oportunidade Empresarial + Porto Seguro → nasce com 7 etapas → criar
uma de Saúde PME 3–29 + SulAmérica → nasce com 12 etapas → criar uma de Vida → usa o genérico, com aviso
→ o protocolo aparece no formato NG-2026-…
**No banco:** tabela de oportunidades e tabelas com as etapas e os itens copiados para cada oportunidade.

---

## Prompt 8 — Ficha da oportunidade

```
Crie a página da FICHA DA OPORTUNIDADE.

Cabeçalho: protocolo, cliente (com link para a ficha), produto, seguradora, responsável comercial, valor estimado, data prevista, situação e etapa atual.

Trilha de etapas: todas as etapas em ordem, mostrando concluídas (com data e quem concluiu), a atual (destacada, com o responsável) e as futuras.

Checklist da etapa atual:
- itens do tipo Documento: botão para anexar o arquivo, mostrando nome, data e quem anexou, com opção de baixar;
- itens do tipo Informação: campo para preencher no formato definido;
- itens do tipo Confirmação: caixa para marcar;
- obrigatórios marcados com asterisco;
- é possível consultar o checklist das etapas já concluídas (somente leitura).

Interações: registrar contato com o cliente (tipo: Ligação, WhatsApp, E-mail, Reunião ou Outro; data; resumo), em lista da mais recente para a mais antiga.

Linha do tempo: tudo o que aconteceu na oportunidade (criação, conclusão de etapa, troca de responsável, documentos, interações), com data, hora e pessoa.

Campo "Nº da proposta/apólice no Quiver".

Os arquivos anexados devem ficar guardados de forma privada, acessíveis só para quem pode ver a oportunidade.
```

**Conferência rápida:** abrir a oportunidade Empresarial → ver a trilha de 7 etapas → anexar um PDF
num item de Documento → preencher um item de Informação → registrar uma interação → tudo aparece na
linha do tempo.
**No banco:** tabelas de documentos, interações e histórico; e uma pasta de arquivos (storage) criada.

---

## Prompt 9 — Movimento: concluir etapa, passar o bastão, ganhar ou perder

```
Crie o MOVIMENTO da oportunidade na ficha.

1) Botão "Concluir etapa":
- só funciona se todos os itens obrigatórios da etapa atual estiverem preenchidos, anexados ou marcados; se faltar algo, mostre a lista do que falta;
- ao concluir, registra data e quem concluiu, e a próxima etapa vira a atual;
- o responsável da próxima etapa é definido pela regra da etapa: se for "responsável comercial da oportunidade", vai para ele; se for "fila de equipe + função", a etapa fica na fila dessa equipe e função, e qualquer pessoa dessa fila pode clicar em "Assumir";
- o novo responsável (ou todas as pessoas da fila) recebe uma notificação no sino: "A oportunidade NG-… chegou para você na etapa …".

2) Botão "Voltar etapa": pede o motivo (texto obrigatório), volta para a etapa anterior e registra na linha do tempo.

3) Etapa de ganho: quando a etapa marcada como "etapa de ganho" é concluída, a oportunidade passa para a situação "Ganha — em finalização" e pede para confirmar a seguradora, o valor fechado (R$) e o nº da proposta. As etapas seguintes continuam normalmente (por exemplo, as de pós-venda). Ao concluir a última etapa, a situação passa a ser "Concluída".

4) Botão "Marcar como perdida" (em qualquer etapa): pede o motivo (lista de motivos do tipo perda, obrigatória) e uma observação. A situação passa a "Perdida".

5) Botão "Arquivar" (só Administrador e Gestor). Não existe botão de excluir oportunidade.

6) O sino de notificações mostra as notificações não lidas, com link para a oportunidade, e permite marcar como lida.

Tudo isso aparece na linha do tempo.
```

**Conferência rápida:** na oportunidade Empresarial, tentar concluir a etapa 1 sem os obrigatórios →
aparece a lista do que falta → preencher e concluir → passa para a etapa 2 → numa de Saúde PME, chegar
até a etapa 10 (pode ser com dados de teste) → ela cai na fila do Pós-venda e quem é do pós-venda recebe
a notificação → marcar outra oportunidade como perdida: o motivo é obrigatório.
**No banco:** tabela de notificações.

---

## Prompt 10 — Minha fila, Quadro, lista de Oportunidades e Painel

```
Agora preencha as páginas de visão.

1) MINHA FILA (página inicial depois do login):
- seção "Comigo": oportunidades em que eu sou o responsável da etapa atual;
- seção "Fila da minha equipe": etapas que estão na fila da minha equipe e função, com o botão "Assumir";
- para cada uma: protocolo, cliente, produto, etapa atual, há quantos dias está na etapa;
- ordenar pelas que estão há mais tempo na etapa; filtro por produto.

2) OPORTUNIDADES (lista):
- todas as oportunidades que eu posso ver;
- busca por cliente, CPF/CNPJ ou protocolo;
- filtros por situação, produto, seguradora, responsável, equipe e período de criação;
- botão para exportar a lista filtrada em planilha (CSV).

3) QUADRO:
- escolher um modelo de fluxo; as colunas são as etapas desse fluxo e os cartões são as oportunidades em andamento naquela etapa;
- cartão com protocolo, cliente, responsável, valor estimado e dias na etapa;
- filtros por responsável e equipe; clicar no cartão abre a ficha.

4) PAINEL (inicial):
- oportunidades em andamento por etapa e por responsável;
- ganhas e perdidas no mês, em quantidade e valor;
- principais motivos de perda do mês;
- filtros por equipe e período.

Cada pessoa só vê nessas páginas as oportunidades que pode ver: Administrador vê todas; Gestor, as das suas equipes; Colaborador, as que são dele, as em que ele foi responsável por alguma etapa e as da fila da sua equipe.
```

**Conferência rápida:** a Minha fila mostra as oportunidades de teste → o Quadro do fluxo Empresarial
mostra os cartões na coluna certa → buscar pelo protocolo encontra → o Painel mostra números que batem
com o que foi feito nos testes → exportar CSV baixa um arquivo.

---

## Depois dos 10 prompts

1. Avise no grupo: **“Tronco concluído”**, com prints das páginas Minha fila, ficha da oportunidade e
   Configurações → Fluxos.
2. Passe para as SPECs: abra o Maestro com o link do repositório de contexto e peça
   **“execute a próxima task”**. A primeira task (T1.01) é a **auditoria do tronco**: o Maestro confere
   o que o Skip criou e prepara o mapa para os galhos. A Louise conduz essa task sozinha no Maestro.

## Observações do tronco (preencher durante o envio)

| Prompt | O que ficou diferente ou faltou | Corrigido na hora? |
|---|---|---|
| | | |
