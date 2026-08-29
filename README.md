# Painel — Mia

Painel web para a apresentação semanal e mensal aos founders.

**Painel:** `painel/index.html` — abra no navegador ou publique.

## Como atualizar a semana

Tudo que muda toda semana está num único bloco no topo de `painel/index.html`,
entre os comentários `DADOS DA SEMANA` e `fim dos dados da semana`.

Cada métrica tem `atual` e `anterior`; o painel calcula as variações sozinho e
pinta verde ou vermelho conforme o que é bom para aquela métrica (num custo,
cair é bom). Percentual vai como número (`6.79` = 6,79%), dinheiro em reais
(`615.72`).

## Abas

| Aba | O que mostra | De onde vêm os dados |
|---|---|---|
| Visão geral | Veredito, indicadores, investimento por canal, funil | todas |
| OpenAI Ads | Impressões, cliques, conversões, CPA, série diária | Windsor · openai_ads |
| Meta Ads | Criativos por objetivo, Connect Rate | Windsor · facebook |
| Google Ads | Campanhas comparadas com o período anterior, termos | Windsor · google_ads |
| Landing Page | Rolagem, tempo ativo, cliques mortos, por dispositivo | Clarity + GA4 |
| Negócio | Cadastros, onboarding, contas ativas, GMV | Metabase |
| Próximos passos | Decisões da semana | manual |

## Documentos

- [`docs/plano-painel.html`](docs/plano-painel.html) — plano de construção
- [`docs/descoberta-fontes.md`](docs/descoberta-fontes.md) — o que cada fonte entrega,
  validado com chamadas reais em 29/08/2026

## Próximo passo

Ligar a coleta automática: uma rotina agendada puxa os números pelo Windsor,
Meta Ads e Metabase e reescreve o bloco de dados antes da reunião.
