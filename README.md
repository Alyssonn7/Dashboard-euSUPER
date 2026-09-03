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

1. Escolha o **Período** e o **Comparar com** na barra de cima — ou use os
   atalhos **Semana fechada** e **Mês fechado**, que preenchem as quatro datas.
   Mexer no período principal reencaixa a comparação na janela anterior de
   mesmo tamanho.
2. Clique em **Puxar dados**. O painel consulta as fontes ao vivo e se redesenha.

A análise mensal é o mesmo fluxo: "Mês fechado" põe o último mês-calendário
completo contra o anterior. Comparar meses de durações diferentes (setembro tem
30 dias, agosto 31) é esperado — a linha de estado nomeia os meses em vez de
acusar janelas de tamanhos diferentes. O histórico das fontes alcança o fim de
2025, então dá para comparar qualquer par de meses desde então (com um buraco
conhecido em novembro/2025 no Google Ads).

A barra de estado, embaixo das datas, diz sempre se o que está na tela é o
retrato gravado no arquivo, o resultado da consulta que você acabou de fazer, ou
uma consulta anterior salva no navegador.

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
