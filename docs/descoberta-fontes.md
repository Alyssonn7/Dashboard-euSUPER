# Descoberta de fontes de dados — 29/08/2026

Resultado de chamadas reais às APIs. Base para o plano do painel.

## Google Ads — Windsor `google_ads`, conta 326-604-5511
**Funciona por completo.** Reproduz o PDF de 20/08 com exatidão.

- Campos: `date, week, year_month, campaign, campaign_id, advertising_channel_type,
  ad_group_name, search_term, keyword_text, impressions, clicks, cpc, ctr, conversions,
  cost_per_conversion, cost, conversion_rate, conversions_signup, video_trueview_views,
  video_quartile_p25_rate/p50/p100, currency`
- Validação: Pesquisa 12/08–20/08 = 3.565 impr / 242 cliques / 5 conv / R$615,7155 — bate 100%.
  Período anterior (05/08–11/08) e baseline dos deltas (29/07–04/08) também batem.
- Histórico: jul/2025 → hoje. **Buraco: nov/2025 não retorna linhas.**
- YouTube vem como `advertising_channel_type = DEMAND_GEN` (não "VIDEO").
- `ctr` e `conversion_rate` vêm como **fração** (0,0712 = 7,12%).
- `cost_per_conversion` vem `null` (não 0) quando não há conversão.

### Divergências encontradas contra o PDF
- **PMAX-01 (VÍDEO DUDA) gastou R$181,03 em 12/08–20/08 e não aparece em nenhum slide.**
  Gasto real do Google no período = R$821,17 (615,72 Search + 181,03 PMax + 24,42 Demand Gen).
- O slide de YouTube diz 12/08–20/08 mas compara **21/08 vs 20/08**.

## Meta Ads — Windsor `facebook`, conta 604915332642452
**Funciona quase por completo.**

- Separação de objetivo confirmada: `adsset_optimization_goal` = `PROFILE_VISIT` vs `OFFSITE_CONVERSIONS`.
- Validado contra o PDF: gasto R$654,83, 114.711 impressões, CTR 3,06%, CPM R$54,73,
  78 cliques no link, 45 LP views, Connect Rate 57,69% — todos batem.
- **Pegadinha:** o campo `ctr` é CTR de todos os cliques. O PDF usa `website_ctr_link_click`.
- **Não reproduzível:** "5.610 visitas ao perfil / R$0,12". `instagram_profile_visits` soma **306**
  nos mesmos 6 anúncios e mesma janela. Janela de atribuição não altera. Reconciliar com o Ads Manager.
- Não existe campo genérico `results`/`cost_per_result` — a coluna tem que ser montada por objetivo.
- `thumbnail_url` funciona (HTTP 200) mas é **URL assinada que expira em ~5 dias** → espelhar na geração.
- `ad_name` não é único entre campanhas — sempre trazer `ad_id` ou `campaign` junto.
- `get_fields` sem filtro devolve 838 campos e estoura o limite de tokens — sempre passar `fields`.

## Metabase — Mia Production (db 4)
**Funciona por completo para produto.** É a camada que falta no relatório atual.

- Tabelas: `professional`, `professional_account`, `event_type`, `event_scheduled`,
  `event_client`, `checkout_session`, `professional_crm_account`.
- Números em 29/08/2026: 2.920 profissionais (2.805 não deletados); onboarding FINISHED 2.132 (76%);
  **conta ACTIVE apenas 105 (3,6%)**; 37.522 agendamentos; 489 checkout sessions (270 SUCCEEDED);
  200 pagamentos; MAU 119.
- GMV por semana de pagamento: 20/07 R$659 · 27/07 R$1.037 · 03/08 R$1.112 · 10/08 R$1.365 ·
  17/08 R$751 · 24/08 R$1.313.
- **`checkout_session.price->>'amount'` está em CENTAVOS.**
- **Zero atribuição:** `professional` não tem utm_source/campaign/gclid/fbclid.
- `mia_ads` (db 5) está congelada em 26/04/2026, só Facebook — inútil para o relatório.
- Questions oficiais excluem IDs de teste; a question 50 (New Users by Week) **não** aplica esse filtro.

## GA4 — Windsor `googleanalytics4`, propriedade 511677134
**Parcial. Um bloqueio estrutural.**

- **`SignUp` é 100% `(not set)`** — 190 eventos em 30d, nenhum atribuível a canal.
  Causa raiz: chegam por RudderStack server-side, sem `session_start` nem parâmetros de origem.
- **Gap de rastreamento: zero eventos SignUp de 15/08 a 23/08** — em cima da janela do relatório.
- 38% das sessões (4.721 de 12.465) são bucket fantasma `(not set)` com bounce 100%.
- Campo `conversions` é inutilizável: `page_view` está marcado como key event.
- Cross-domain miaapp.com.br ↔ web.miaapp.com.br não configurado → self-referral.
- Útil: sessões por canal, landing page por URL, `form_start`, cliques no CTA de WhatsApp por canal.
- Tráfego de dev poluindo a base (localhost, lovable.app, tagassistant).

## Microsoft Clarity — Windsor `microsoft_clarity`, conta 1347
**Janela de 3 dias. Exige snapshot diário.**

- Confirmado empiricamente: pedir 12/08–20/08 retorna erro. `date_preset=last_7d` retorna erro.
- O campo `date` é **cosmético** — é só a data final pedida, não a data do dado.
- **Rate limit de ~10 chamadas por projeto por dia.** Após ~7 queries a conta parou de responder.
- Não entrega heatmap, gravações, nem ranking de botões por elemento.
- Entrega de bônus que o PDF não tem: quebra por **device** (mobile scroll 11,38% vs PC 32,37%)
  e por **canal**.
- URLs não normalizadas (243 URLs para uma LP) — fazer `split('?')[0]`.
- Agregados por dimensão não reconciliam com o total — usar cada corte isoladamente.

## OpenAI Ads
**Não conectado.**

- O connector `openai_ads` **existe** no catálogo do Windsor, mas não há conta conectada
  (`No openai_ads account for user mia_marketing was found`).
- Auth manual: exige `account_name` + `api_key` (OpenAI Ads Manager → Settings → API keys,
  uma key por conta de anúncio).
- Proxy que funciona hoje: GA4 `source_medium = "chatgpt / paid"`.
  17–20/08 = 353 sessões (PDF reporta 404 cliques — coerente).
  A campanha aparentemente foi pausada em 23/08 (cai para 1-2 sessões/dia).
- O proxy não dá impressões, gasto, CPC nem CPA.

## Automação já existente
Três Routines ativas publicando em Slack `#eusuper-tráfego` (C0BT9RML3PV):
diária 06:05, semanal segunda 07:00, mensal dia 1º 07:30. A diária rodou com sucesso em 29/08.
Já usam Meta Ads MCP, Google Ads e Clarity via Windsor, e mantêm a série histórica do Clarity
em `claude/serie-historica-trafego.md` para contornar o limite de 3 dias.
