# De onde vem cada número

Atualizado em 31/08/2026. Janela: **24/08–30/08** contra **17/08–23/08**
(semanas fechadas, segunda a domingo).

## O painel não consulta nada

O painel é um arquivo HTML estático. Mudar as datas nele **não dispara consulta
nenhuma** — só troca os rótulos e recalcula o aviso de duração. Os números estão
gravados no bloco `DADOS`, no topo do arquivo.

Uma página publicada como Artifact também não consegue chamar o Windsor nem o
Metabase: a capacidade de conector não está liberada para esta conta. Por isso a
coleta é feita fora do painel e o resultado é escrito no arquivo.

## Como a coleta é feita

Não é uma query só. É uma chamada por fonte, por janela.

| Fonte | Como | Tipo |
|---|---|---|
| Google Ads | `mcp__Windsor_ai__get_data` com `connector:"google_ads"`, conta `326-604-5511`, `date_from`/`date_to` | REST do Windsor, não SQL |
| Meta Ads | idem, `connector:"facebook"`, conta `604915332642452` | REST do Windsor |
| Sessões na LP | idem, `connector:"googleanalytics4"`, propriedade `511677134` | REST do Windsor |
| Clarity | idem, `connector:"microsoft_clarity"`, conta `1347`, `date_preset:"last_3d"` | REST do Windsor |
| Negócio | `mcp__Metabase__execute_sql` na base `Mia Production` (id 4) | SQL de verdade |

São 9 chamadas por semana: 4 do Windsor × 2 janelas, mais 1 SQL que já devolve as
duas janelas.

## Estado de cada fonte

| Fonte | Janela que entrega | Estado |
|---|---|---|
| Google Ads | exata, qualquer período, ~13 meses de histórico | completo |
| Meta Ads | exata, por anúncio | completo |
| Negócio (Metabase) | exata, coorte por data de cadastro | completo |
| Sessões na LP (GA4) | exata | completo, mas sem atribuição de cadastro por canal |
| Clarity | **só os últimos 3 dias** | parcial — não cobre a janela |
| OpenAI Ads | — | **não conectado** |

## Duas coisas que faziam os números não fecharem

**Janelas misturadas.** Na primeira versão do painel cada cartão vinha de um
período diferente: mídia de 12/08–20/08, Clarity de 26/08–28/08, sessões de
22/08–28/08, negócio das semanas de 16 e 23/08. Somar isso num funil produz taxas
que não querem dizer nada. Agora tudo vem da mesma janela, e o Clarity — que não
consegue — está isolado num cartão com a data dele escrita.

**Coorte misturada com plataforma.** Cadastros, onboarding e ativação de conta são
da **coorte**: quem entrou dentro da janela. Agendamentos, pagamentos e GMV são da
**plataforma inteira**, incluindo profissionais antigos. Misturar os dois é o que
fazia aparecer "41 pagamentos" embaixo de "2 contas ativadas". Agora são dois
blocos separados.

Também vale saber: **sessões na LP são mais que cliques em anúncio** (1.781 contra
775), porque a página recebe tráfego direto, orgânico e de IA. O funil marca essa
etapa com asterisco — ela não é uma cascata pura.

E a **ativação de conta demora**: quem se cadastrou nesta semana ainda não teve
tempo de completar o cadastro bancário. Comparar a coorte nova com a anterior
sempre favorece a mais antiga (2 contra 13). É maturidade de coorte, não queda
de desempenho.

## O que foi jogado fora

A versão anterior do painel tinha número estimado em: série diária do OpenAI Ads,
coluna "anterior" do Meta, Performance Max anterior, Clarity anterior, e contas
ativas. Tudo isso saiu. Onde não há dado real, o painel agora escreve que não há.
