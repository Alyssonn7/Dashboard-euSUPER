# Painel — euSUPER! (ex-Mia)

Painel semanal e mensal para a reunião com os founders.

**Painel:** `painel/index.html` — um arquivo, sem build, sem dependência.
A raiz do repositório redireciona para `/painel/`.

**Painel v2 (funil de cadastro):** `painel-v2/index.html` — convive com a v1,
que segue intacta. Detalhes na seção [Painel v2](#painel-v2--funil-de-cadastro).

## Painel v2 — funil de cadastro

Segue o manual de identidade da euSUPER!: faixa azul no topo (degradê oficial,
barra do "!" a 22,5° e o círculo laranja), títulos em minúsculas, botão em caixa
alta e ícones da marca. Fonte Gilroy quando instalada, Outfit no lugar.

**Tema claro e escuro.** O claro é o do manual (fundo branco). O escuro é
derivado do azul da marca (fundo `#0A1A30`, cartões `#10243F`), com o cinza
`#87B1DB` e o azul claro `#D7FFF7` como cor de texto. Sem escolha, o painel
segue o tema do claude.ai ou do sistema; o botão **modo escuro / modo claro** no
topo fixa a escolha neste navegador (`v2:tema`).

Formato de resumo: filtros num cartão que sobe sobre a faixa, quatro números
principais (investimento, cadastros, custo por cadastro, taxa de conversão),
quatro mini blocos numa linha, na ordem do funil (CPM, onboarding concluído,
custo por onboarding e ativação D7) e o funil na largura toda.

**Os quatro principais são só do tráfego pago.** Cadastros = conversões que
Google, Meta e OpenAI atribuem a si (somadas); custo por cadastro =
investimento ÷ esses cadastros; taxa de conversão = esses cadastros ÷ cliques.
A linha fecha a conta (investimento ÷ cadastros = custo por cadastro). O custo
da etapa Cadastros do funil e a dica do gráfico por dia usam o mesmo custo.
Onboarding, custo por onboarding e ativação continuam sobre o total de
cadastros do Metabase (com ou sem anúncio), porque nenhuma fonte grava de onde
veio cada cadastro; o funil mostra esse total como "Cadastros (total)". CTR e CPC saíram dos mini blocos:
continuam no funil, como a taxa de Cliques e o custo por clique. Cada etapa do funil mostra o total, a comparação, a
taxa sobre a etapa anterior e quanto custou (CPM no topo, depois investimento ÷
etapa). No celular o funil vira uma lista de barras. Abaixo, investimento e
cadastros por dia.

**Duas abas no topo:** **Funil de cadastro** (o que está descrito aqui) e
**Evolução mensal**. O painel lembra a última aba aberta; o link com `#evolucao`
no fim abre direto nela.

**Evolução mensal** — a base inteira mês a mês, últimos 12 meses, tudo do
Metabase e sem as contas de teste. Quatro cartões do mês atual com o % de
crescimento, um gráfico por métrica e a tabela mês a mês com a variação:

| Métrica | Como é contada |
|---|---|
| Usuários cadastrados | Contas existentes no fim do mês, sem as apagadas |
| Contas ativas | Profissionais com pelo menos um atendimento realizado no mês |
| Serviços realizados | Agendamentos confirmados (`CONFERENCE_SCHEDULED`) cujo horário já passou; cancelados não contam. O sistema não registra quando o cliente falta |
| Valor dos serviços | Soma do preço que o profissional cadastrou no serviço (`event.et_price`). Não é pagamento confirmado; serviços com preço R$ 0 não somam |

O crescimento é contra o mês anterior. O mês atual, que ainda não acabou, é
comparado com os mesmos dias do mês anterior (ex.: 1 a 19 de outubro contra 1
a 19 de setembro; para o total de usuários, o total de hoje contra o do mesmo
dia do mês passado). Uma consulta só ao Metabase traz os 13 meses (o mais
antigo é a base do crescimento do primeiro) e a linha de comparação; o
resultado fica salvo no navegador (`v2:evolucao`).

**Só campanhas de cadastro entram no funil.** As de divulgação ficam fora de
tudo — investimento, impressões, cliques, taxas, custos, gráficos e lista de
campanhas — e o cartão do funil lista o que ficou fora no período e quanto
gastou.

| Canal | Cadastro (entra) | Divulgação (fica fora) |
|---|---|---|
| Meta | objetivo de conversão (`OUTCOME_SALES`, `OUTCOME_LEADS`) | tráfego, engajamento, reconhecimento (ex.: visita ao perfil do Instagram, `OUTCOME_TRAFFIC`) |
| Google | campanhas de pesquisa (`SEARCH`) | vídeo, Demand Gen, Display, Performance Max |
| OpenAI | todas | — |

Com essa regra, agosto perde R$ 1.673 (nove campanhas de tráfego no Meta e
duas de vídeo no Google: investimento de R$ 7.040,06 para R$ 5.366,76) e
setembro perde a "YT - Motion Horizontal" (R$ 237,75).

**Teste A/B** (abaixo dos gráficos por dia): um **placar de jogo**, feito para
a reunião semanal. As duas páginas são os times (a vencedora ganha o selo
"venceu"), o placar fica no meio e cada canal é uma rodada com um "cabo de
guerra" puxado para quem converteu mais. Cada rodada vence quem teve a **taxa
de conversão maior**, exatamente como os dados lidos nos gerenciadores mostram;
a marca diz a margem: **por pouco** (menos de 2 p.p.) ou **com folga** (2 p.p.
ou mais). Embaixo, só duas linhas: **por quê** e **próximo**. As taxas ficam em
`TESTE_AB` e aparecem ao passar o mouse em cada rodada. O bloco **não segue os
filtros**: mostra o período do próprio teste. Hoje: Teste A/B 03 com as duas
semanas (06/10 a 20/10): A (miaapp.com.br) 3 × 0 B — com folga no Meta, por
pouco no Google e na OpenAI.

**Comparativo semanal** (opção do "Período de visualização"): semanas de
**quarta a terça**, as mesmas do relatório semanal do marketing. A semana atual
é a que contém ontem; se ainda está em andamento, vai até ontem e aparece como
"Semana atual · parcial" (ex.: na terça 27/10, 21/10 a 26/10, 6 de 7 dias),
sempre contra a semana anterior inteira — como o slide. Na quarta, a semana
atual está fechada: em 21/10, 14/10 a 20/10 contra 07/10 a 13/10. As setas
‹ › andam de semana em semana. Nesse modo a comparação fica travada em "Semana anterior";
ao sair, volta a que estava escolhida.

**Filtros:** período (esse mês, comparativo semanal, últimos 7/14/30 dias, semana passada, mês
passado, personalizado — sempre até ontem), comparação (mesmo período do mês
anterior, período anterior ou nenhuma), fonte de tráfego e campanha. Trocar
filtro não consulta nada: o painel guarda as linhas por dia e por campanha e
recalcula na hora. Só busca de novo quando o período pedido ainda não foi
carregado.

**Dois modos de funil:**

| Filtro | Funil | De onde vem o cadastro |
|---|---|---|
| Todos os canais | Impressões → Cliques → Cadastros → Onboarding concluído → 1º cliente em 7 dias | Metabase (todos os cadastros, com ou sem anúncio) |
| Um canal ou uma campanha | Impressões → Cliques → (Visitas na página, só Meta) → Cadastros | Conversão reportada pela própria plataforma |

Onboarding e ativação só existem no total porque nenhuma fonte grava a
origem do cadastro.

**Mesmo método do relatório semanal do marketing** (conferido em 21/10: semana
07–13/10 = 503 cadastros, 40,6% de onboarding, 8,2% de ativação, igual ao
slide):

- dia do cadastro no **horário de Brasília** (o banco grava em UTC);
- contas **excluídas depois continuam contando** como cadastro;
- **1º cliente em 7 dias (a etapa que antes se chamava "Conta ativa") = pelo
  menos um agendamento de cliente com e-mail e telefone
  diferentes dos da profissional**, criado nos 7 dias depois do cadastro
  (`event_client` ligado a `event_scheduled.professional_id`, `created_at` do
  cliente dentro da janela, rascunho `DRAFT` não conta, telefone comparado pelos
  9 últimos dígitos). A profissional agendando a si mesma para testar não conta.

**Ativação D7 = contas ativas ÷ todos os cadastros com 7 dias completos.** Como
a janela leva 7 dias para fechar, a taxa só usa os cadastros que já completaram
a janela, e só aparece quando pelo menos metade dos cadastros do recorte já
fechou; antes disso o cartão mostra "—" e "Fecha em DD/MM". No **comparativo
semanal**, a semana recente ainda está aberta, então o cartão usa a semana
mais nova que já fechou (a escolhida, a anterior ou a de antes) contra a semana
antes dela, como o slide ("Cadastros de 07/10 a 13/10 · 41 de 503"); o painel
busca essas semanas extras junto. No funil, a etapa 1º cliente em 7 dias de uma semana
ainda aberta mostra "—" e "D7 fecha em DD/MM" e, embaixo, a mesma taxa do
cartão: "última fechada: 8,2% (07–13/10)".
Enquanto ainda há cadastros dentro da janela de 7 dias, a contagem da etapa
vai subir: no lugar da seta de variação aparece **"parcial"** (com o "antes" ao
lado), para não parecer queda. O nome "Conta ativa" saiu do funil porque a aba
Evolução mensal usa "Contas ativas" com outro sentido (atendeu no mês). O cartão se chama "Ativação D7" ("de todos os
cadastros"). No funil, a etapa 1º cliente em 7 dias mostra a contagem (que ainda sobe
nos períodos recentes), a ativação D7 no meio da faixa ("dos cadastros (D7)") e,
embaixo, a taxa de quem fez o onboarding: só quem concluiu o onboarding e ativou
÷ quem concluiu o onboarding (~19–25%). Essa etapa não usa o "passo da etapa
anterior", porque quem pula o onboarding também pode ativar — antes, ativos ÷
onboarding dava ~21–27% e era lido como se fosse a ativação D7. A altura das faixas do funil é logarítmica, para caber de
centenas de milhares de impressões a dezenas de contas, com piso de 30% da
primeira faixa, para o % e as duas linhas de texto caberem nas etapas finais.

**Origem dos cadastros** (bloco abaixo do funil, só em "Todos os canais"): de
onde veio cada cadastro segundo o **nosso banco**, não a plataforma de anúncio.
O site grava a origem desde **23/10/26 às 8h59** (tabela `professional_tracking`:
UTM, gclid, fbclid, página de entrada e site de onde a pessoa veio); antes disso
o bloco avisa que não há dado, e os cadastros de 23/10 feitos antes das 8h59
ficam de fora (a nota diz quantos).

| Origem | Regra (na ordem) |
|---|---|
| Google Ads | tem gclid, ou UTM `google` com meio pago (cpc, ppc, paid…) |
| Meta Ads | UTM `meta`/`facebook`/`instagram` com meio pago (paid_social, cpc…), fbclid com campanha, ou fonte `meta` sem meio vinda do Facebook/Instagram (anúncio com UTM incompleta) |
| OpenAI Ads | UTM `openai` |
| Orgânico → Bio do Instagram | o próprio Instagram marca o link da bio (`utm_content=link_in_bio`) |
| Orgânico → Busca | sem anúncio, veio do Google ou de outro buscador |
| Orgânico → Redes sociais | sem anúncio, veio de Instagram, Facebook, TikTok etc. (ou fbclid sem campanha) |
| Orgânico → Direto | sem UTM e sem site de origem (digitou o endereço ou link salvo) |
| Orgânico → Outros sites | qualquer outro site, inclusive o ChatGPT sem anúncio; e anúncio de outra rede |
| Sem origem | o site não gravou a origem desse cadastro |

O bloco tem uma barra 100% (Google, Meta, OpenAI, Orgânico e Sem origem
hachurado; cores validadas para daltonismo) e uma tabela: cadastros, parcela
(com "antes X%" quando o período anterior também tem origem), onboarding,
ativação D7 por origem (a coluna aparece quando os cadastros do período fecham
os 7 dias) e **custo real por cadastro** = investimento das campanhas de
cadastro do canal ÷ cadastros que o banco registrou vindos dele. Passando o
mouse no custo, aparece o que a plataforma diz ter trazido, para comparar. No
celular, a tabela fica com origem, cadastros, parcela e custo real.

Para a reunião, o bloco se explica sozinho: cada linha do orgânico e o "Sem
origem" têm um mini-texto embaixo do nome (ex.: "Direto — digitou o site ou
veio de WhatsApp e e-mail"), e abaixo do total há uma **observação** com o
orgânico e o sem origem do período em palavras (quantos, o maior grupo, quantos
dos sem origem não terminaram o onboarding e o provável motivo: a pessoa cria a
conta e sai antes de o site anotar a origem, que é gravada uns 30 segundos depois)
e a caixa **Em investigação**, com o que o time de desenvolvimento e o de
marketing estão fazendo. Esse texto fica em `ORIGEM_INVESTIGA`, no topo do código.

**Selo laranja de atenção** = a métrica piorou 10% ou mais contra a comparação
(custo que subiu, taxa que caiu); o número e a borda do cartão também ficam
laranja.

**Dica de cada cartão (passar o mouse, ou tocar no "i" no celular):** com o
mouse parado no cartão, o resto da página desfoca e o cartão fica em
destaque, com a dica logo abaixo. A dica é curta: nome, fórmula em uma
linha e, em "Todos os canais", o valor de cada canal (Google, Meta, OpenAI)
com a variação, mais a linha **Geral**. O geral é a soma de tudo, não a média
dos três (CPM = investimento total ÷ impressões totais × 1.000). Onboarding,
custo por onboarding e ativação só existem no total.

**Fontes (quatro chamadas por intervalo):** Windsor.ai `get_data` para
`google_ads` (326-604-5511), `facebook` (604915332642452) e `openai_ads` (sem
pino de conta), todas por dia e campanha; Metabase `execute_sql` na base 4 com
cadastros, onboarding e ativação por dia de cadastro (Brasília), sem os IDs
de teste e com as contas excluídas (inclui maduros, ativos maduros, onboarding
maduros e ativos com onboarding entre os maduros; dados guardados no navegador
na versão `v:5`).
O conector `openai_ads` do Windsor **agora traz `conversions`** — agosto bate
com o export do Ads Manager (117). O GA4 não está mais conectado no Windsor, por
isso "Visitas na página" só aparece no Meta (landing page view do pixel).

**Ao vivo e retrato:** dentro do claude.ai o painel atualiza sozinho ao abrir
(se os dados tiverem mais de 30 minutos) e pelo botão Atualizar. Fora dele
mostra o retrato gravado no arquivo — 01/08 a 12/10/2026, conferido campanha a
campanha contra os totais do Windsor e do Metabase. O último resultado ao vivo
fica salvo no navegador (`v2:dados`), junto com os filtros (`v2:filtros`).

## Identidade visual

O front segue o **Manual da Identidade Visual euSUPER! (2026)**: azul
`#008CF5` (gradiente até `#1760B9`) como cor institucional, laranja `#F9902A`
na ação principal, verde `#05B27C`, cinza `#87B1DB` e azul claro `#D7FFF7`
como fundo. Todos os valores vivem nos tokens do `:root` no topo do
`painel/index.html`. A tipografia oficial é a **Gilroy**; como ela é
licenciada, o painel carrega a **Poppins** (equivalente geométrica do Google
Fonts) — a Gilroy vem primeiro na pilha `--fonte` e assume automaticamente
onde estiver instalada. O símbolo (check + exclamação a 22,5°), o wordmark e
os ícones do menu vêm da iconografia do manual, desenhados como SVG no
próprio arquivo. As contas de anúncio, domínios e nomes de fonte de dados
ainda são os da operação atual (miaapp.com.br, Metabase "Mia Production") e
trocam quando a migração de marca chegar ao produto.

## Como usar

1. A barra de cima diz o que está na tela: "**Agosto de 2026** comparado com
   **Julho de 2026**". Clique no período para abrir a edição das quatro datas
   (Aplicar datas fecha; Cancelar ou Esc desfaz), ou use os atalhos **Semana
   passada** e **Mês passado**. Mexer no período principal reencaixa a
   comparação na janela anterior de mesmo tamanho.
2. Clique em **Puxar dados**. O painel consulta as fontes ao vivo e se redesenha.

A análise mensal é o mesmo fluxo: "Mês passado" põe o último mês-calendário
completo contra o anterior. Comparar meses de durações diferentes (setembro tem
30 dias, agosto 31) é esperado — a linha de estado nomeia os meses em vez de
acusar janelas de tamanhos diferentes. O histórico das fontes alcança o fim de
2025, então dá para comparar qualquer par de meses desde então (com um buraco
conhecido em novembro/2025 no Google Ads).

A linha de estado, embaixo da frase, diz sempre se o que está na tela é o
retrato gravado no arquivo, o resultado da consulta que você acabou de fazer, ou
uma consulta anterior salva no navegador — e avisa em laranja quando as datas
escolhidas ainda não foram puxadas.

O painel **lembra as quatro datas e o último "Puxar dados"** no próprio navegador
(localStorage): fechar a página, atualizar, ou receber uma versão nova do painel
não apaga nada. Quando o formato dos dados muda numa versão nova, só o resultado
salvo é descartado — as datas ficam.

## Cinco seções

| Seção | O que mostra |
|---|---|
| **Google Ads** | Impressões, cliques, CTR, CPC, conversões, taxa de conversão, custo por conversão, investimento — mais a tabela por campanha |
| **Meta Ads** | Dois grupos, por objetivo de campanha: **Campanha de cadastro** (alcance, impressões, frequência, CPM, cliques no link, CTR, chegaram na página, conversões, taxa, custo por conversão, investimento) e **Campanha de visita ao perfil do Instagram** (alcance, impressões, frequência, CPM, cliques, CTR, visitas ao perfil, custo por visita, investimento) — e, abaixo de cada grupo, o **top 3 de criativos** dele (menor custo por conversão / por visita), com a miniatura de cada anúncio |
| **OpenAI Ads** | Só o export do Ads Manager (`campaigns.csv`, 17/08–31/08), a pedido: conversões, custo por conversão, taxa, páginas vistas, investimento, impressões, cliques, CTR, CPC, CPM. A tabela por campanha e a entrega ao vivo do Windsor não aparecem nesta aba, a pedido |
| **Landing Page** | 100% Microsoft Clarity, do export mensal: visitas, visitantes novos, rolagem média, tempo ativo, cliques de saída, cliques em "Entrar", no celular e velocidade — mais de onde vieram as visitas. Só isso, a pedido |
| **Visão geral** | Agosto fechado em duas camadas: um cartão por canal (logo, investimento e fatia do total) e barras empilhadas de 100% por métrica mostrando onde foi o dinheiro e de onde vieram os cadastros. A tabela completa e o bloco do Metabase saíram a pedido |

As abas **Resumo** (cinco números, o funil e o GMV semanal) e **Próximos passos**
(cartões de ação) existem no código mas estão ocultas — o modelo de apresentação
atual não as usa. Para reativar, remova a flag `oculta:true` da entrada
correspondente na lista `VISTAS` do `painel/index.html`. O botão Imprimir também
saiu a pedido.

Cada bloco de métrica traz três coisas: o valor, a variação contra a janela
anterior, e **uma linha dizendo o que aquela métrica significa** — "de 100 que
viram, quantos clicaram", "quanto custou cada cadastro". É o que deixa a tela
legível para quem não vive de tráfego.

O cartão **"O que esses números dizem"** foi retirado de todas as abas a pedido —
a apresentação aos founders não usa.

## Dados de mês fechado que vêm de export (não do botão)

Três blocos do painel são **fechamentos de mês** carregados de arquivos, e não
mudam com "Puxar dados": as conversões do OpenAI Ads (`OPENAI_MANAGER`), a aba
Landing Page (`CLARITY_AGOSTO`) e a aba Visão geral (`VISAO_AGOSTO`). Cada um
diz na tela de onde veio e que período cobre. Para virar o mês, mande os exports
novos (CSV de campanhas do Ads Manager e CSV do painel do Clarity) e peça a
atualização — a Visão geral eu fecho com os mesmos Windsor e Metabase do painel.

### Conversões do OpenAI Ads

O conector `openai_ads` do Windsor não tem campo de conversão. Por isso o painel
carrega um **export do Ads Manager** (`campaigns.csv`) na constante
`OPENAI_MANAGER` do `painel/index.html`, com as campanhas de agosto. O export
foi conferido dia a dia no Windsor: impressões, cliques e gasto de cada campanha
batem no centavo com a soma de 17/08 a 31/08 — só as conversões são informação
nova. Esse bloco **não muda com "Puxar dados"**; para atualizar, exporte o CSV
de campanhas do Ads Manager e peça a atualização da constante.

## Nada de métrica guardada pela metade

CTR, CPC, CPM, frequência, taxa de conversão e custo por conversão **não são
gravados** — são calculados na hora, a partir das contagens brutas. Assim não
existe número que não fecha com o vizinho.

Quando a janela anterior é zero, a variação diz **"vinha de 0"** em vez de um
travessão: 24 conversões contra zero é a história, não um dado ausente.

## De onde vem cada número

| Bloco | Fonte | Chamada |
|---|---|---|
| Google Ads | Windsor · `google_ads` · conta 326-604-5511 | `get_data` |
| Meta Ads | Windsor · `facebook` · conta 604915332642452 | `get_data` |
| OpenAI Ads | Windsor · `openai_ads` (sem pino de conta — o id muda a cada reconexão) | `get_data` |
| Visitas na página | Windsor · `googleanalytics4` · propriedade 511677134 | `get_data` |
| Comportamento na página | Windsor · `microsoft_clarity` · conta 1347 | `get_data` |
| Cadastros, onboarding, pagamentos, GMV | Metabase · Mia Production | `execute_sql` |

São **treze chamadas** por consulta: Google, OpenAI e GA4 × duas janelas, o Meta ×
três chamadas por janela (uma sem dimensão, para o alcance vir deduplicado como
no Ads Manager; uma por campanha com o objetivo, que separa cadastro de visita ao
perfil; e uma por anúncio para a tabela), mais um SQL que devolve as duas
janelas. O Clarity não é mais consultado ao vivo — a Landing Page é só o export.

No Meta, o alcance de cada grupo só é deduplicado quando aquele grupo foi o
único a gastar na janela (aí vale o alcance da conta). Quando os dois grupos
rodaram, o Windsor não deduplica por grupo: o alcance é a soma das campanhas
do grupo, e o bloco diz isso na linha de explicação.

O GMV por semana é série gravada — as seis semanas não vêm da consulta.

## O que cada fonte não entrega

- **OpenAI Ads** está conectado e lendo, mas não tem campo de conversão: traz
  impressões, cliques, gasto, CPC e CPM — nunca CPA. As conversões vêm do export
  manual do Ads Manager (seção acima). E a API recusa janelas que terminam hoje:
  o fim tem que ser até ontem, no fuso da conta de anúncio.
- **Microsoft Clarity** só devolve os últimos 3 dias pela API. Por isso a Landing
  Page usa o export mensal do painel do Clarity e não consulta a API ao vivo. O GA4
  continua sendo consultado, mas não aparece na Landing Page — a aba é 100% Clarity.
- **Origem do cadastro** não existe em lugar nenhum. Por isso o custo por cadastro
  aparece consolidado, e não por canal.

## Quando uma fonte falha

Cada bloco falha sozinho e mostra o que fazer — conexão expirada, conector não
adicionado, permissão negada, limite de chamadas, erro da plataforma de origem.
Os outros blocos continuam na tela.

Fora do claude.ai a consulta ao vivo não existe: o painel avisa e mostra o
retrato gravado.

## Documentos

- [`docs/plano-painel.html`](docs/plano-painel.html) — plano de construção
- [`docs/descoberta-fontes.md`](docs/descoberta-fontes.md) — o que cada fonte entrega
- [`docs/procedencia.md`](docs/procedencia.md) — como a coleta é feita
- [`docs/publicar.md`](docs/publicar.md) — publicar numa URL própria
