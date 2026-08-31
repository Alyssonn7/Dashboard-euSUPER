# Painel — Mia

Painel semanal e mensal para a reunião com os founders.

**Painel:** `painel/index.html` — um arquivo, sem build, sem dependência.
A raiz do repositório redireciona para `/painel/`.

## Como usar

1. Escolha o **Período** e o **Comparar com** na barra de cima. Mexer no período
   principal reencaixa a comparação na janela anterior de mesmo tamanho.
2. Clique em **Puxar dados**. O painel consulta as fontes ao vivo e se redesenha.

A barra de estado, embaixo das datas, diz sempre se o que está na tela é o
retrato gravado no arquivo ou o resultado da consulta que você acabou de fazer.

## Quatro seções, nesta ordem

| Seção | O que mostra |
|---|---|
| **Resumo** | Cinco números, uma frase, o funil em quatro etapas e o GMV por semana |
| **Canais** | Google, Meta e OpenAI: gasto, cliques, custo por clique e por resultado |
| **Detalhe** | Campanhas e anúncios — para quando perguntarem |
| **Decisões** | O que fica combinado para a semana que começa |

Os cinco números da abertura: **Investimento**, **Cadastros novos**,
**Custo por cadastro**, **Pagamentos** e **GMV**. Nada mais. Founder entende os
cinco sem explicação.

## De onde vem cada número

| Bloco | Fonte | Chamada |
|---|---|---|
| Google Ads | Windsor · `google_ads` · conta 326-604-5511 | `get_data` |
| Meta Ads | Windsor · `facebook` · conta 604915332642452 | `get_data` |
| OpenAI Ads | Windsor · `openai_ads` · conta 284 | `get_data` |
| Visitas na página | Windsor · `googleanalytics4` · propriedade 511677134 | `get_data` |
| Cadastros, onboarding, pagamentos, GMV | Metabase · Mia Production | `execute_sql` |

São **nove chamadas** por consulta: quatro fontes do Windsor × duas janelas, mais
um SQL que já devolve as duas.

O GMV por semana é série gravada — as seis semanas não vêm da consulta.

## O que cada fonte não entrega

- **OpenAI Ads** não tem campo de conversão. Vai trazer impressões, cliques,
  gasto, CPC e CPM — nunca CPA.
- **Microsoft Clarity** ficou fora: a API só devolve os últimos 3 dias, então não
  cobre nenhuma janela escolhida. Precisa de retrato diário acumulado.
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
