# Painel — Mia

Painel web para a apresentação semanal e mensal aos founders.

**Painel:** `painel/index.html` — abra no navegador, ou publique numa URL
seguindo [`docs/publicar.md`](docs/publicar.md). A raiz redireciona para `/painel/`.

## Como atualizar a semana

Tudo que muda toda semana está num único bloco no topo de `painel/index.html`,
entre os comentários `DADOS DA SEMANA` e `fim dos dados da semana`.

Cada métrica tem `atual` e `anterior`; o painel calcula as variações sozinho e
pinta verde ou vermelho conforme o que é bom para aquela métrica (num custo,
cair é bom). Percentual vai como número (`6.79` = 6,79%), dinheiro em reais
(`615.72`).

## Seções

| Seção | O que mostra | De onde vêm os dados |
|---|---|---|
| A semana | Veredito, GMV por semana, funil, indicadores, três taxas, criativos | todas |
| OpenAI Ads | Impressões, cliques, conversões, CPA, série diária | Windsor · openai_ads |
| Meta Ads | Criativos por objetivo, Connect Rate | Windsor · facebook |
| Google Ads | Campanhas comparadas com o período anterior, termos | Windsor · google_ads |
| A página | Rolagem, tempo ativo, cliques mortos, por dispositivo | Clarity + GA4 |
| O negócio | Cadastros, onboarding, contas ativas, GMV | Metabase |
| O que vem | Decisões da semana | manual |

## Documentos

- [`docs/plano-painel.html`](docs/plano-painel.html) — plano de construção
- [`docs/descoberta-fontes.md`](docs/descoberta-fontes.md) — o que cada fonte entrega,
  validado com chamadas reais em 29/08/2026

## Próximo passo

Ligar a coleta automática: uma rotina agendada puxa os números pelo Windsor,
Meta Ads e Metabase e reescreve o bloco de dados antes da reunião.

## Visual

Moldura em verde-mata sobre fundo menta, casca branca arredondada, menu lateral
com ícones e cartões brancos — seguindo a referência de layout escolhida.

O verde escuro é **moldura**, não cor de dado: ele reprova o piso de croma do
validador de paleta, ou seja, vira cinza quando usado como barra ou linha de
gráfico. As séries usam verde `#0E8F63`, laranja `#D07C1A` e violeta `#8C4FBF`
no tema claro, e `#14A070` / `#C67B22` / `#8E72D0` no escuro — as duas paletas
passam os seis testes (banda de luminosidade, piso de croma, separação para
daltonismo, piso de visão normal e contraste com a superfície).

Tipos: Plus Jakarta Sans nos títulos e números, IBM Plex Sans no corpo.

Tem alternador de tema claro/escuro, filtro de texto por seção e botão de
imprimir com folha de estilo própria para PDF.

## Comportamento de app

A página não rola. A barra lateral, o cabeçalho e a faixa de período ficam
fixos, e só a área de conteúdo rola por dentro — do mesmo jeito que um sistema
web. Trocar de seção volta a rolagem para o topo.

Larguras:

| Largura | Comportamento |
|---|---|
| acima de 1240px | menu com rótulos, duas colunas nos blocos |
| 1240px a 1080px | menu recolhe para ícones, item ativo em pastilha branca |
| abaixo de 1080px | blocos passam a uma coluna |
| abaixo de 760px | menu vira faixa horizontal no topo, cartões e gráficos compactos |
| abaixo de 420px | indicadores empilham em coluna única |

Verificado em 1440, 1180, 900 e 390px: a página não rola em nenhum eixo, o
conteúdo rola por dentro, e nenhum elemento passa da borda direita.

## As duas janelas de data

A faixa de período mostra **as duas** datas, sempre visíveis: o período atual e
o de comparação. Mexer no período atual reencaixa a comparação na janela
imediatamente anterior, do mesmo tamanho; o botão "Janela anterior" refaz esse
encaixe a qualquer momento. A comparação também pode ser editada à mão.

Ao lado aparece a duração das duas janelas. Quando os tamanhos não batem
(9 dias contra 7, por exemplo) o aviso fica vermelho — comparar janelas de
durações diferentes distorce todos os deltas sem dar sinal.
