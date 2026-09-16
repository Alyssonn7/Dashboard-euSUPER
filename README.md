# Painel — euSUPER! (ex-Mia)

Painel semanal e mensal para a reunião com os founders.

**Painel:** `painel/index.html` — um arquivo, sem build, sem dependência.
A raiz do repositório redireciona para `/painel/`.

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
   **julho de 2026**". Clique em **Escolher datas** (ou no próprio período) para
   abrir a edição — Aplicar datas fecha, Cancelar ou Esc desfaz. Os atalhos são
   **Semana passada**, **Este mês** (do dia 1 até ontem, contra o mesmo trecho do
   mês passado) e **Mês passado**. Mexer no período principal reencaixa a
   comparação na janela anterior de mesmo tamanho.
2. Clique em **Puxar dados**. O painel consulta as fontes ao vivo e se redesenha.

### Dois modos: com comparação e sem

Dentro da edição há a caixa **Comparar com outro período**. Ligada (o padrão), o
painel funciona como sempre: cada bloco mostra os dois números lado a lado com a
seta de variação. Desligada, o painel passa a mostrar **um período só** — a barra
vira uma frase simples ("1 a 6 de setembro"), os blocos ficam de uma coluna, sem
seta, e a consulta cai de dezoito para **onze chamadas**. Serve para olhar uma
semana, um punhado de dias ou um mês isolado sem inventar uma base de comparação.

O modo fica guardado no navegador junto com as datas. Religar a comparação
reencaixa a janela anterior de mesmo tamanho, porque o período pode ter mudado
enquanto ela estava desligada — a edição continua aberta para ajustar.

A análise mensal é o mesmo fluxo: "Mês passado" põe o último mês-calendário
completo contra o anterior. Comparar meses de durações diferentes (setembro tem
30 dias, agosto 31) é esperado — a linha de estado nomeia os meses em vez de
acusar janelas de tamanhos diferentes. O histórico das fontes alcança o fim de
2025, então dá para comparar qualquer par de meses desde então (com um buraco
conhecido em novembro/2025 no Google Ads).

Um aviso sobre o dia corrente: se a janela escolhida terminar **hoje**, o último
dia vem incompleto (em 04/09, às 14h, o Instagram tinha 511 de alcance contra
~3.000 de um dia fechado). Os atalhos **Semana passada** e **Mês passado** sempre
usam períodos fechados, então no fluxo normal isso não aparece.

A linha de estado, embaixo da frase, diz sempre se o que está na tela é o
retrato gravado no arquivo, o resultado da consulta que você acabou de fazer, ou
uma consulta anterior salva no navegador — e avisa em laranja quando as datas
escolhidas ainda não foram puxadas.

O painel **lembra as quatro datas, o modo de comparação e o último "Puxar
dados"** no próprio navegador (localStorage): fechar a página, atualizar, ou
receber uma versão nova do painel não apaga nada. Quando o formato dos dados muda
numa versão nova, só o resultado salvo é descartado — as datas ficam.

A exceção é o **carimbo do retrato** (`RETRATO.carimbo`): quando um retrato novo é
gravado com um período diferente, o painel adota esse período **uma vez**, mesmo
que o navegador tenha datas salvas — senão um retrato recém-fechado abriria com as
datas de outra semana. Depois dessa primeira vez as datas que você escolher voltam
a mandar.

O resultado salvo (`mia:vivo`) é guardado sob uma chave que junta `VERSAO_DADOS` e
o carimbo, então **um retrato novo descarta sozinho o resultado velho**. Isso
existe porque o contrário deu problema uma vez: com a aba Instagram oculta, um
"Puxar dados" gravou junto o retrato do Instagram daquele momento; quando a aba
voltou com dados novos, o resultado salvo continuou válido e mostrava os números
de agosto por baixo dos rótulos de setembro.

## Seis seções

| Seção | O que mostra |
|---|---|
| **Google Ads** | Impressões, cliques, CTR, CPC, conversões, taxa de conversão, custo por conversão, investimento, a tabela por campanha — e o grupo **Disputa no leilão**: parcela de impressões, perdido por classificação, perdido por orçamento, no topo da página, mais a lista de domínios que apareceram nas mesmas buscas |
| **Meta Ads** | Dois grupos, por objetivo de campanha: **Campanha de cadastro** (alcance, impressões, frequência, CPM, cliques no link, CTR, chegaram na página, conversões, taxa, custo por conversão, investimento) e **Campanha de visita ao perfil do Instagram** (alcance, impressões, frequência, CPM, cliques, CTR, visitas ao perfil, custo por visita, investimento) — e, abaixo de cada grupo, o **top 3 de criativos** dele (menor custo por conversão / por visita), com a miniatura de cada anúncio |
| **OpenAI Ads** | Só o export do Ads Manager, a pedido — agora **semana contra semana** (08–14/09 contra 01–07/09): conversões, custo por conversão, taxa, investimento, impressões, cliques, CTR, CPC e CPM. Os blocos de páginas vistas saíram porque o export novo não traz mais essa coluna. A tabela por campanha e a entrega ao vivo do Windsor não aparecem nesta aba, a pedido |
| **Instagram** | Orgânico, do conector `instagram` do Windsor (Instagram Insights). Alcance, visualizações, frequência, novos seguidores, contas que interagiram, toques nos links do perfil e seguidores agora — mais interações totais, taxa de engajamento, curtidas, comentários, salvamentos e compartilhamentos, e o **top 5 de posts** do período. O perfil **@usemiaapp** foi ligado no Windsor em 04/09/2026 e a aba já lê ao vivo; se a conexão cair, ela volta a mostrar o passo a passo da ligação |
| **Landing Page** | 100% Microsoft Clarity, do export do painel, **semana contra semana**, enxuta a pedido para **cinco métricas**: visitas, seguiram para o produto, rolagem média, tempo ativo e velocidade — mais de onde vieram as visitas, com o número da semana anterior ao lado. Visitantes novos, cliques em "Entrar" e "no celular" saíram da tela, mas os campos continuam em `CLARITY_SEMANAS` e voltam removendo uma linha do render |
| **Visão geral** | Cobre a **janela cheia** do painel (01–14/09), não a semana de cima. Um bloco de acumulado no topo com as conversões somadas e o custo médio por cadastro; abaixo, duas camadas: um cartão por canal (logo, investimento e fatia do total) e barras empilhadas de 100% por métrica mostrando onde foi o dinheiro e de onde vieram os cadastros. A tabela completa e o bloco do Metabase saíram a pedido |

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

Três blocos do painel são **fechamentos de período** carregados de arquivos, e
não mudam com "Puxar dados": as conversões do OpenAI Ads (`OPENAI_MANAGER`), a
aba Landing Page (`CLARITY_SEMANAS`) e a aba Visão geral (`VISAO_SEMANA`). Cada
um diz na tela de onde veio e que período cobre. Para virar a semana ou o mês,
mande os exports novos (CSV de campanhas do Ads Manager e CSV do painel do
Clarity) e peça a atualização — a Visão geral eu fecho com o mesmo Windsor do
painel.

A Visão geral é puxada pedindo o **período inteiro de uma vez**, e não somando
as semanas: o alcance do Meta não se soma. Em 01–14/09 a conta deu **45.321**
deduplicado, enquanto somar as duas semanas daria 53.951 e contaria duas vezes as
8.630 pessoas que viram nas duas. Gasto, impressões e cliques conferem com a soma.

**Cuidado com a janela do export do Clarity.** Na virada de 15/09 o arquivo da
semana nova veio marcado `09/07 00:00 – 09/14 23:59`, ou seja 07 a 14/09: oito
dias, com o dia 07/09 repetido no arquivo anterior. O painel não corrige isso —
ele **avisa na tela**, mostra os rótulos reais de cada export nas colunas e põe a
média por dia ao lado, que é a leitura justa. Ao exportar, confira se a data
inicial é mesmo a que você quer.

### Conversões do OpenAI Ads

O conector `openai_ads` do Windsor não tem campo de conversão. Por isso o painel
carrega um **export do Ads Manager** na constante `OPENAI_MANAGER` do
`painel/index.html`, com uma janela em `atual` e outra em `anterior`. Cada export
é conferido contra o Windsor antes de entrar: em 15/09, nas quatro linhas das duas
semanas, impressões e cliques bateram exatamente e o gasto diferiu um centavo por
campanha (arredondamento do export) — só as conversões são informação nova. Esse bloco **não muda com "Puxar dados"**; para atualizar, exporte o CSV
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
| Instagram orgânico | Windsor · `instagram` · @usemiaapp (17841476511713959; sem pino de conta — só existe um perfil) | `get_data` |
| Visitas na página | Windsor · `googleanalytics4` · propriedade 511677134 | `get_data` |
| Comportamento na página | Windsor · `microsoft_clarity` · conta 1347 | `get_data` |
| Cadastros, onboarding, pagamentos, GMV | Metabase · Mia Production | `execute_sql` |

São **vinte e uma chamadas** por consulta com comparação (treze sem ela). As cinco do
Instagram só entram enquanto a aba estiver visível — aba oculta não desenha nada,
então consultá-la seria gasto puro, e com ela escondida a consulta cai para treze
e sete: Google, OpenAI e GA4 × duas janelas, o Meta ×
três chamadas por janela (uma sem dimensão, para o alcance vir deduplicado como
no Ads Manager; uma por campanha com o objetivo, que separa cadastro de visita ao
perfil; e uma por anúncio para a tabela), o Instagram × cinco (o perfil dia a
dia nas duas janelas, os posts do período, a contagem de seguidores e os novos
seguidores — esta última cobre as duas janelas de uma vez e é separada de
propósito, veja abaixo), mais um SQL que devolve as duas janelas. O Clarity não é mais consultado ao vivo — a
Landing Page é só o export.

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
- **Instagram orgânico** tem cinco limites, todos confirmados contra a API em
  04/09/2026 e todos ditos na tela:
  - **Alcance não fecha por período.** A API só entrega alcance e contas
    engajadas **por dia**, então o número do período é a soma dos dias e quem viu
    em dois dias conta duas vezes. Não existe alcance deduplicado de período,
    diferente do Meta Ads.
  - **O alcance é da conta, não só do orgânico.** Nos dias de campanha ele sobe
    muito acima do alcance somado dos posts (7.146 num dia em que os posts
    alcançaram 63), então lê-se como alcance da conta.
  - **Novos seguidores só dos últimos 30 dias.** `follower_count` é recusado para
    qualquer janela mais antiga que isso, e derruba a consulta inteira junto — por
    isso ele vai numa chamada separada, que cobre as duas janelas de uma vez. Se
    falhar, só aquele bloco fica com travessão e diz por quê.
  - **Seguidores sem histórico.** É a contagem do momento da consulta, não do fim
    do período; o bloco se chama "Seguidores agora" por isso.
  - **Salvamentos podem ser negativos.** O Instagram desconta quem tirou o
    salvamento: a janela 17–23/08 fechou em −1.
  - **Métrica de post não se recorta por data.** A janela escolhida decide quais
    posts entram na tabela (pela data de publicação), mas o alcance, as
    visualizações e as interações de cada post são o total acumulado até o
    momento da consulta. Conferido: pedir só 28/08 devolve o mesmo alcance 98
    do reel que pedir agosto inteiro. O card diz isso.

  Além disso: as impressões foram substituídas por **visualizações** pelo próprio
  Instagram; **stories** só existem por 24 horas, então não entram em relatório de
  mês fechado; **seguidor ganho por post** não é medido em reels (vem nulo, e a
  coluna mostra travessão em vez de zero); e a miniatura de cada post vem de
  domínio externo, que o artifact bloqueia — o quadradinho traz o tipo do post e
  abre o post no Instagram, e as miniaturas entram embutidas quando valer a pena,
  como as do Meta.
- **Estatísticas de leilão do Google** saem pela metade. A *parcela de
  impressões* vem inteira (quanto da busca disponível o anúncio ganhou, e se
  perdeu por orçamento ou por classificação) e é puxada **sem dimensão de
  campanha**, porque é razão e não contagem. Já o relatório de concorrentes só
  devolve o **domínio**: o Windsor expõe `auction_insight_domain`, mas nenhuma
  das métricas que o acompanham — sobreposição, taxa de superação, parcela de
  cada concorrente. O próprio Google recusa a combinação ("unsupported
  metrics"), e esses números existem apenas dentro do painel do Google Ads.
  Cuidado com o **valor-sentinela**: abaixo de 10% a API devolve `0,0999` e
  acima de 90% devolve `0,9`. Conferido dia a dia — o "topo absoluto" veio
  `0,0999` nos sete dias enquanto os outros variavam. O painel escreve "menos de
  10%" em vez de fingir 9,99%.
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
