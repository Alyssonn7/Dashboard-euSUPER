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
2. **Não precisa clicar em nada.** Aberto dentro do claude.ai, o painel consulta
   Google Ads, Meta Ads e OpenAI Ads no Windsor **sozinho** — ao abrir e sempre que
   as datas mudam (Aplicar, Enter ou um atalho). A linha de estado mostra
   "Atualizando…" enquanto consulta. Se o que está na tela já é das mesmas datas e
   tem menos de 30 minutos, ele não consulta de novo. **Puxar dados** continua lá
   para forçar uma consulta na hora.

   Fora do claude.ai não há consulta: aparece o último retrato gravado.

   **Landing Page continua manual**: os dados do Clarity e do teste A/B chegam
   por arquivo e entram no painel à mão.

### Dois modos: com comparação e sem

Dentro da edição há a caixa **Comparar com outro período**. Ligada (o padrão), o
painel funciona como sempre: cada bloco mostra os dois números lado a lado com a
seta de variação. Desligada, o painel passa a mostrar **um período só** — a barra
vira uma frase simples ("1 a 6 de setembro"), os blocos ficam de uma coluna, sem
seta, e a consulta cai de onze para **seis chamadas**. Serve para olhar uma
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
| **Google Ads** | Impressões, cliques, CTR, CPC, conversões, taxa de conversão, custo por conversão, investimento total no Google e a tabela por campanha. O funil tem como base **Total: Pesquisa**, não a linha de conta: as conversões saem todas da Pesquisa, e quando há campanha de YouTube no ar a linha de conta soma engajamentos de vídeo aos cliques e afunda a taxa de conversão. O dinheiro do YouTube não some — aparece no bloco de investimento total e na tabela por campanha |
| **Meta Ads** | Dois grupos, por objetivo de campanha: **Campanha de cadastro** (alcance, impressões, frequência, CPM, cliques no link, CTR, chegaram na página, conversões, taxa, custo por conversão, investimento) e **Campanha de visita ao perfil do Instagram** (alcance, impressões, frequência, CPM, cliques, CTR, visitas ao perfil, custo por visita, investimento) — e, abaixo de cada grupo, o **top 3 de criativos** dele (menor custo por conversão / por visita), com a miniatura de cada anúncio |
| **OpenAI Ads** | **Ao vivo pelo Windsor** desde 05/10, com as datas da barra: conversões, custo por conversão, taxa, investimento, impressões, cliques, CTR, CPC e CPM. O export do Ads Manager (`OPENAI_MANAGER`) ficou só de reserva, para quando não há consulta. A tabela por campanha não aparece nesta aba, a pedido |
| **Instagram** | Orgânico, do conector `instagram` do Windsor (Instagram Insights). Alcance, visualizações, frequência, novos seguidores, contas que interagiram, toques nos links do perfil e seguidores agora — mais interações totais, taxa de engajamento, curtidas, comentários, salvamentos e compartilhamentos, e o **top 5 de posts** do período. O perfil **@usemiaapp** foi ligado no Windsor em 04/09/2026 e a aba já lê ao vivo; se a conexão cair, ela volta a mostrar o passo a passo da ligação |
| **Landing Page** | **Dois testes A/B, cada um num bloco independente**, o mais recente em cima — a pedido, um não cita o outro. O **Teste 02** põe a miaapp.com.br (LP Luy) contra a lp2 (LP Mia, versão nova): conversão por canal (`AB_LP2.canais`), mapas de calor com a vencedora marcada e a caixa “O que esse teste ensinou”, cujo veredito é calculado das taxas. A **miaapp.com.br venceu nos três canais**. Abaixo, o **Teste 01**: `miaapp.com.br` (A) contra `lp1.miaapp.com.br` (B), com uma captura de cada página embutida, a **taxa de conversão por canal** nos três gerenciadores de anúncio e o **comportamento dentro da página** pelo Clarity: os dois **mapas de calor** lado a lado e a caixa **“O que esse teste ensinou”**, com a leitura em três frases e os números dentro do texto. Os blocos soltos de toques saíram a pedido — os campos continuam em `AB_LP`. B venceu nos três canais. A versão anterior da aba — Clarity semana contra semana, cinco métricas e de onde vieram as visitas — continua inteira em `vLPSemanal()`, com os dados em `CLARITY_SEMANAS`: volta trocando `r:vLP` por `r:vLPSemanal` na lista `VISTAS` |
| **Visão geral** | **Calculada das três abas de anúncio** a cada consulta — nunca discorda delas. Quando as duas janelas são vizinhas (duas semanas seguidas), cobre a **janela cheia** (ex.: 15–28/09); quando não são, só a atual. Google entra com o investimento da conta inteira e impressões, cliques e cadastros da Pesquisa, como na aba dele. Canal que falhar não entra pela metade: sai da conta e a tela diz qual e por quê. Um bloco de acumulado no topo com as conversões somadas e o custo médio por cadastro; abaixo, duas camadas: um cartão por canal (logo, investimento e fatia do total) e barras empilhadas de 100% por métrica mostrando onde foi o dinheiro e de onde vieram os cadastros. A tabela completa e o bloco do Metabase saíram a pedido |
| **Outros** | Registro do que foi montado fora dos anúncios. Hoje traz o **fluxo de atendimento automático do WhatsApp**, feito no ManyChat: o passo a passo em cinco itens (condição de horário → resposta → vídeos → áudio depois de 7 minutos → conversa marcada como aberta) e a captura da tela do construtor. Não é métrica: o ManyChat não está conectado a nenhuma fonte de dados do painel, e a aba diz isso |

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

## O que ainda vem de arquivo

**Desde 05/10 só a Landing Page é manual** (teste A/B em `AB_LP`, Clarity semanal
em `CLARITY_SEMANAS`). OpenAI Ads e Visão geral passaram a sair da consulta ao
vivo; `OPENAI_MANAGER` e `VISAO_SEMANA` ficaram como **reserva**, mostrados só
quando não há consulta (fora do claude.ai). O texto abaixo é o histórico de como
esses blocos eram fechados.

Três blocos do painel eram **fechamentos de período** carregados de arquivos, e
não mudavam com "Puxar dados": as conversões do OpenAI Ads (`OPENAI_MANAGER`), a
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

**Atualização de 05/10:** o conector ganhou o campo `conversions`. Conferido
contra o export do Ads Manager campanha por campanha: 61+26 = 87 em 22–28/09 e
43+10 = 53 em 15–21/09, iguais; impressões e cliques idênticos; gasto com um ou
dois centavos de diferença. A aba passou a sair da consulta. O resto desta seção
é histórico.

Até 05/10 o conector `openai_ads` do Windsor não tinha campo de conversão. Por isso o painel
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
| Instagram orgânico | ~~Windsor · `instagram`~~ — **desconectado em 05/10** (aba oculta) | — |
| Visitas na página | ~~Windsor · `googleanalytics4`~~ — **desconectado em 05/10**, só alimentava o Resumo oculto | — |
| Comportamento na página | ~~Windsor · `microsoft_clarity`~~ — **desconectado em 05/10**; a Landing Page usa o export | — |
| Cadastros, onboarding, pagamentos, GMV | Metabase · Mia Production | `execute_sql` |

São **onze chamadas** por consulta com comparação (seis sem ela): Google e OpenAI × duas
janelas, o Meta × três chamadas por janela (uma sem dimensão, para o alcance vir
deduplicado como no Ads Manager; uma por campanha com o objetivo, que separa cadastro
de visita ao perfil; e uma por anúncio para a tabela), mais um SQL que devolve as duas
janelas. Desde 05/10 o plano Basic do Windsor cobre três fontes, e ficaram só os três
canais de anúncio: GA4, Instagram e Clarity saíram da consulta. As funções continuam
escritas (`qSessoes`, `qInsta*`) e voltam quando a fonte for reconectada.

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
  Page usa o export do painel do Clarity e não consulta a API ao vivo. O GA4 saiu
  da consulta em 05/10, quando foi desconectado do Windsor.
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
- **O teste A/B mistura duas fontes, e a aba diz isso.** As taxas de conversão
  por canal são **números informados**, lidos por Alysson em cada gerenciador de
  anúncio: não há export delas no repositório, e nada no painel as recalcula. Já
  exibições e toques vêm de dois exports do Clarity, um por página, e cobrem
  **23–28/09 — seis dias**, contra os sete do teste de anúncios. Por isso as duas
  coisas ficam em blocos separados e os volumes nunca somam com os dos
  gerenciadores. A página A recebeu mais tráfego que a B, então só as colunas de
  proporção se comparam. A leitura de que a página B converte mais porque
  concentra os toques num único CTA está marcada na tela como **leitura**, não como
  medição: o Clarity mede toque, não cadastro.
- **As quatro capturas vão embutidas em base64** (JPEG, ~235 KB no total): as duas
  páginas e os dois mapas de calor. O artifact bloqueia imagem de domínio externo, então um `src` apontando
  para o site não apareceria.
- **Origem do cadastro** não existe em lugar nenhum. Por isso o custo por cadastro
  aparece consolidado, e não por canal.

## Quando uma fonte falha

**O Windsor não avisa quando o plano trava as leituras.** Em 29/09/2026 a conta
`mia_marketing` estava no plano **Free** (1 conta conectada permitida) com **6
conectadas**, e a API passou a responder com **tudo zerado**, escondendo o aviso
dentro de um campo de texto (o nome da campanha, o id da conta). Sem tratamento,
o painel mostraria "Dados ao vivo" com zero em todos os blocos — pior do que não
mostrar nada. A função `linhas()` agora procura esse aviso em qualquer campo de
texto da resposta e transforma em falha explícita, e a linha de estado passou a
dizer **quantas das consultas falharam**, não só quando todas falham.

**Em 05/10/2026 a conta passou para o plano Basic** (3 fontes) e foram desconectados
GA4, Instagram e Clarity. No mesmo dia o `get_data` do Windsor **mudou de formato**,
e o painel foi ajustado para os dois:

- A resposta passou de `{result:[...]}` para `{status:"done", data:[...]}`. O
  `linhas()` antigo só lia `result`, então leria lista vazia e **somaria zero** — o
  mesmo número falso que o aviso de plano produzia. Agora lê os dois, e resposta sem
  lista nenhuma vira falha, nunca zero.
- A primeira resposta pode vir `{status:"pending", poll_after_seconds}` sem dado
  nenhum. O `chamar()` repete a **mesma** chamada (o Windsor devolve o mesmo job) até
  `done`, com teto de 12 tentativas.
- Erros agora também chegam **dentro** da resposta (`{error:"..."}`) em vez de falhar a
  chamada. Os dois que já apareceram ganham tradução — conta não selecionada e
  permissão do Google Ads perdida.
- O aviso de plano pausado agora usa os números **da própria resposta** ("há 5 fontes
  e o plano Basic cobre 3") em vez de texto fixo.

**O caminho ao vivo do Google passou a usar a mesma base do retrato gravado.** Até
05/10 o `qGoogle()` somava todas as campanhas no funil e agrupava a tabela por tipo
de campanha — os dois erros que já tinham sido corrigidos no retrato. Com o YouTube
no ar, isso derrubava a taxa de conversão de 22–28/09 de 17,26% (a do gerenciador)
para 14,48%. Agora o funil é só a Pesquisa, a conta inteira vai para o bloco de
investimento total e a tabela lista as campanhas pelo nome. Pela API o campo de
cliques é clique de verdade também no YouTube (119, contra 3.884 “interações” no
export), então numa consulta ao vivo a coluna se chama **Cliques** e a ressalva do
YouTube some. Conferido contra os dois exports: a Pesquisa bate até o centavo nas
duas semanas.

Testado com um Windsor simulado no navegador: os oito formatos de resposta e o
botão "Puxar dados" inteiro com o Google desconectado e Meta/OpenAI devolvendo os
números reais de 22–28/09.


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
