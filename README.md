# LanzaTech

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
