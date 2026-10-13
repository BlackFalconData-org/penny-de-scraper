# Penny Germany Scraper - Weekly Offers & Prices

Extract structured data from [penny.de](https://penny.de) — Collect the current and next-week penny.de flyer with offer price and crossed-out price plus discount percent and unit price per kg or litre. Filter by category or local PENNY flyer area and track new or changed offers between scheduled runs.

**[Penny Germany Scraper - Weekly Offers & Prices on Apify →](https://apify.com/blackfalcondata/penny-de-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Multiple input modes** — weekly offers or full catalog or keyword search. Switch modes without re-scraping.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N products (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from penny.de on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from penny.de.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "mode": "weeklyOffers",
  "maxResults": 100,
  "onlyOffers": false,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | enum | `"weeklyOffers"` | Weekly offers = the current and next flyer. Full catalog = every flyer category (same data, all categories). Search = only products whose name matches your keywords. |
| `query` | string | — | Search mode only: product keywords matched against the product name. Use a JSON array for several, e.g. ["kaffee","butter"]. |
| `categories` | array | `[]` | Category slugs to collect, e.g. kuehlregal, obst-und-gemuese, top-angebote. Leave empty for every category. |
| `regions` | array | `[]` | Optional PENNY selling-region codes. Each region is collected separately and every record carries its sellingRegion. Leave empty for the nationwide flyer. |
| `maxResults` | integer | `100` | Maximum products in total (0 = everything in the flyer). |
| `startUrls` | array | `[]` | Optional: paste penny.de offer or category page URLs. Each URL becomes its own task; results are de-duplicated by product. |
| `onlyOffers` | boolean | `false` | Drop flyer products shown at their normal price (no reduction, no PENNY-app price). |
| `priceMin` | number | — | Lowest offer price (EUR) to keep. |
| `priceMax` | number | — | Highest offer price (EUR) to keep. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state — new offers, ended offers and price changes only. stateKey is optional: it defaults to a value derived from your search inputs (mode, categories, keywords, regions and filters) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked offer universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit products that have not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental, also emit offers that are no longer in the flyer. Needs a complete scan, so maxResults does not limit the scan in this case. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches an expired product from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 41-field schema. Missing values are `null` — never omitted.

- `productId`
- `offerId`
- `slug`
- `title`
- `brand`
- `category`
- `url`
- `portalUrl`
- `imageUrl`
- `price`
- `currency`
- `regularPrice`
- `regularPriceType`
- `discountPercent`
- `savingsAmount`
- `isOnOffer`
- `offerType`
- `promotion`
- `isMultiBuy`
- `requiresAppForPrice`
- `appPrice`
- `appDiscountPercent`
- `appUnitPrice`
- `packText`
- `unitPrice`
- `unitPriceUnit`
- `unitPriceRaw`
- `offerWeek`
- `offerPeriod`
- `offerValidFrom`
- `offerValidUntil`
- `sellingRegion`
- `onlyOnline`
- `alsoOnline`
- `searchQuery`
- `contentHash`
- `detailFetched`
- `scrapedAt`
- `source`
- `offerChange`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "productId": "34a2eae9467a599997a9a96fdeaae81e742ce59e00a13ce68336cae9f3dfd6dd",
  "offerId": "95c19537-e8d8-4052-bab8-0011340250c2",
  "slug": "philadelphia-frischkaesezubereitung",
  "title": "PHILADELPHIA Frischkäse­zubereitung",
  "brand": "PHILADELPHIA",
  "category": "top-angebote",
  "url": "https://www.penny.de/angebote/top-angebote/philadelphia-frischkaesezubereitung",
  "portalUrl": "https://www.penny.de/angebote/top-angebote/philadelphia-frischkaesezubereitung",
  "imageUrl": "https://cdn.penny.de/dam/jcr:23faebc3-dfcd-4f20-bbf0-3e430d3db16f/51414117.png?impolicy=penny&imwidth=800",
  "price": 1.11,
  "currency": "EUR",
  "regularPrice": 2.29
}
```

*Truncated — full records contain 41 fields. See Output fields for the complete schema.*

**[Try Penny Germany Scraper - Weekly Offers & Prices now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/penny-de-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00108 |

See the [actor on Apify](https://apify.com/blackfalcondata/penny-de-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape penny.de?**
Use this actor on Apify to extract structured data from penny.de. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get penny.de data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape penny.de?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check penny.de's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Penny Germany Scraper - Weekly Offers & Prices](https://apify.com/blackfalcondata/penny-de-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
