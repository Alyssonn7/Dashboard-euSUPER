# Painel — Mia

Painel semanal e mensal para a reunião com os founders.

**Painel:** `painel/index.html` — um arquivo, sem build, sem dependência.
A raiz do repositório redireciona para `/painel/`.

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
retrato gravado no arquivo ou o resultado da consulta que você acabou de fazer.

## Seis seções, uma por canal

| Seção | O que mostra |
|---|---|
| **Resumo** | Cinco números, uma frase, o funil e o GMV por semana |
| **Google Ads** | Impressões, cliques, CTR, CPC, conversões, taxa de conversão, custo por conversão, investimento — mais a tabela por campanha |
| **Meta Ads** | Alcance, impressões, frequência, CPM, cliques no link, CTR, chegaram na página, conversões, taxa, custo por conversão, investimento — mais a tabela por anúncio |
| **OpenAI Ads** | Impressões, cliques, CTR, CPC, CPM, investimento |
| **Landing Page** | Visitas na janela escolhida, e o comportamento pelo Clarity |
| **Próximos passos** | Cartões de ação com "por quê" e "o que fazer" |

Cada bloco de métrica traz três coisas: o valor, a variação contra a janela
anterior, e **uma linha dizendo o que aquela métrica significa** — "de 100 que
viram, quantos clicaram", "quanto custou cada cadastro". É o que deixa a tela
legível para quem não vive de tráfego.

Cada aba de canal fecha com **"O que esses números dizem"**: um parágrafo em
português direto, montado a partir dos próprios números da consulta. Muda quando
os dados mudam, e aponta o criativo mais eficiente e o que gastou sem retorno.

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

São **doze chamadas** por consulta: Google, OpenAI e GA4 × duas janelas, o Meta ×
duas chamadas por janela (uma sem dimensão, para o alcance vir deduplicado como
no Ads Manager, e uma por anúncio para a tabela), mais uma do Clarity e um SQL
que devolve as duas janelas.

O GMV por semana é série gravada — as seis semanas não vêm da consulta.

## O que cada fonte não entrega

- **OpenAI Ads** está conectado e lendo, mas não tem campo de conversão: traz
  impressões, cliques, gasto, CPC e CPM — nunca CPA. E a API recusa janelas que
  terminam hoje: o fim tem que ser até ontem, no fuso da conta de anúncio.
- **Microsoft Clarity** só devolve os últimos 3 dias. Ele aparece na aba Landing
  Page com a janela dele marcada em amarelo, e não entra em nenhuma conta que
  dependa do período escolhido.
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
