# Bizi.si Slovenian Company Scraper

Extract structured data from [bizi.si](https://bizi.si) — Get Slovenian company records from the bizi.si registry: name, registration number, tax ID, address, postcode, city, activity, employee size band and coordinates. Search by keyword or paste a search or company URL. Incremental mode returns only new and changed companies.

**[Bizi.si Slovenian Company Scraper on Apify →](https://apify.com/blackfalcondata/bizi-si-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from bizi.si on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from bizi.si.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "restavracija",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all",
  "includeDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Company name, activity or keyword to search the Slovenian business registry for (e.g. "restavracija", "gradbeništvo", "d.o.o."). Use a JSON array for several searches in one run. Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw bizi.si search-results URLs (e.g. https://www.bizi.si/iskanje?q=restavracija). Each URL becomes its own search task; results are merged and deduped by registry number across all URLs. When provided, startUrls REPLACE the query field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null / empty-string / empty-array fields from each output record. Off by default to preserve dataset schema compatibility; turn on to slim output for LLM / agent consumption. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (queries, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit companies that have not changed since the last run (incrementalMode only). |
| `emitExpired` | boolean | `false` | Also emit companies that have disappeared from the search results since the last run (incrementalMode only). |
| `skipReposts` | boolean | `false` | When incremental, skip companies whose content matches a record that expired in a prior run (cross-run re-listing detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED companies. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of companies included in each notification message (1–20). |
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
| `includeDetails` | boolean | `false` | bizi.si search rows already contain every output field, and detail enrichment is not available in this version, so this setting currently has no effect. |
| `lat` | number | — | GPS latitude. Pair with lon. |
| `lon` | number | — | GPS longitude. Pair with lat. |
| `radiusKm` | integer | — | Search radius in kilometres around the centre given by lat and lon. Needs both lat and lon. |

---

## Output fields

Every record returns the same 26-field schema. Missing values are `null` — never omitted.

- `companyName`
- `profileUrl`
- `portalUrl`
- `registrationNumber`
- `taxId`
- `address`
- `postalCode`
- `city`
- `activity`
- `employeeCount`
- `latitude`
- `longitude`
- `sourceUrl`
- `scrapedAt`
- `distanceKm`
- `page`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "companyName": "RESTAVRACIJA IN HOTEL TRI LUČKE d.o.o.",
  "profileUrl": "https://www.bizi.si/RESTAVRACIJA-IN-HOTEL-TRI-LUCKE-D-O-O/",
  "portalUrl": "https://www.bizi.si/RESTAVRACIJA-IN-HOTEL-TRI-LUCKE-D-O-O/",
  "registrationNumber": "8026262000",
  "taxId": "47807164",
  "address": "Sremič 23",
  "postalCode": "8270",
  "city": "Krško",
  "activity": "Gostilne in restavracije",
  "employeeCount": "5 do 9 zaposlenih",
  "latitude": 45.97007,
  "longitude": 15.49105
}
```

*Truncated — full records contain 26 fields. See Output fields for the complete schema.*

**[Try Bizi.si Slovenian Company Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/bizi-si-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002637 |

See the [actor on Apify](https://apify.com/blackfalcondata/bizi-si-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape bizi.si?**
Use this actor on Apify to extract structured data from bizi.si. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get bizi.si data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape bizi.si?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check bizi.si's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Bizi.si Slovenian Company Scraper](https://apify.com/blackfalcondata/bizi-si-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
