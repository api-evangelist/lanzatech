# LanzaTech

LanzaTech (Nasdaq LNZA) is a carbon recycling company that uses gas fermentation to turn waste carbon
into fuels and chemicals. Proprietary bacteria consume carbon-rich off-gases from steel mills,
ferroalloy plants and landfills and produce ethanol and other intermediates, sold as CarbonSmart products
and used by brands including Unilever, Coty and Zara. LanzaTech operates commercial plants in China,
India and Belgium, is building the LanzaTech Porsgrunn CCUS project in Norway with Eramet, and works with
LanzaJet on sustainable aviation fuel.

Backed by: khosla-ventures, qiming — https://lanzatech.com/

## The API

LanzaTech runs no developer program and publishes no API documentation. It does serve `lanzatech.com` on
WordPress with a **fully public, unauthenticated WordPress REST API** at `https://lanzatech.com/wp-json`.

What makes it worth cataloguing is that LanzaTech models itself in that API. Alongside the standard
WordPress types it registers five custom content types:

| Type | Items | What it is |
|---|---|---|
| `news` | 35 | Curated third-party press coverage |
| `board-member` | 14 | Board of directors profiles |
| `download` | 5 | Published reports and life-cycle assessments |
| `employee` | 4 | Leadership team profiles |
| `testimonial` | 1 | Partner and customer quotes |

Plus 107 blog posts, 16 pages, 314 media items, 4 categories and 141 tags.

## Artifacts

| Artifact | Method |
|---|---|
| `openapi/` — OpenAPI 3.0.3, 119 paths, 264 operations | derived from `https://lanzatech.com/wp-json/` |
| `conventions/` — pagination, sparse fieldsets, embedding, versioning | derived + live-observed |
| `errors/` — every error code captured from a real request | derived from live responses |
| `data-model/` — entity and taxonomy graph with item counts | derived from `/wp/v2/types` |
| `authentication/` — WordPress application passwords | derived |
| `lifecycle/`, `conformance/` — what exists and what does not | derived |
| `mcp/` — candidate MCP tool list over the public read surface | derived |
| `skills/` — two agent skills on verified operationIds | generated |
| `llms/`, `overlays/`, `agentic-access/` | generated |
| `well-known/`, `security/` | probed |

## Recorded absences

These were probed and are genuinely not published — no pointer is emitted for any of them:

- No `/.well-known/` documents at all (security.txt, openid-configuration, api-catalog: all 404)
- No `llms.txt`, no OpenAPI, no AsyncAPI, no webhooks, no event surface
- No SDKs or client libraries (npm: 0 results; PyPI: 404)
- No CLI, no sandbox, no UI components, no Postman collection
- No status page, no deprecation policy, no SLA, no API changelog
- No OAuth (HTTP Basic only), so no scopes artifact
- No idempotency mechanism
- No vulnerability disclosure programme, no trust center, no published certifications
- No rate-limit signalling — though a Sucuri WAF fronts the site and intercepts `/wp/v2/users` and
  unauthenticated writes, returning HTML rather than the JSON error envelope
