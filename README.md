# Splunk Observability Cloud

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

Splunk Observability Cloud is the observability platform Splunk built on SignalFx and now runs as part of Cisco: infrastructure monitoring, APM, real user monitoring, synthetics, Log Observer and incident response over OpenTelemetry-native ingest. Its control plane is the largest API surface Splunk operates — 48 OpenAPI documents and 242 operations covering charts, dashboards, detectors, incidents and muting rules, metrics and dimension metadata, SignalFlow, SLOs, org and session tokens, teams, integrations and twenty distinct Synthetics services — alongside a SignalFlow WebSocket/SSE streaming interface and a hosted MCP server for agents.

## Ownership

Part of the Splunk family.

## Contract status — corrected 2026-08-19

A previous pass recorded this provider as a soft-404 farm with no confirmable contract. **That
finding was wrong, and this is the correction.**

What is true: `dev.splunk.com` does answer HTTP 200 with an identical 6,638-byte Next.js shell for
every *asset-style* path — `/openapi.json`, `/llms.txt`, `/.well-known/*`, and any invented path.
Splunk publishes no downloadable OpenAPI file, no `/openapi.json`, no Swagger export and no spec
repository.

What was missed: the *documentation* pages render, and each of the 48 API reference pages embeds
Splunk's own parsed OpenAPI object — `servers`, paths, methods, `operationId`s, parameters,
request and response schemas, and examples — inside its React Server Component payload. All 48
specifications and all 242 operations in `openapi/` were reconstructed from that payload and match
Splunk's own published inventory exactly, spec for spec and operation for operation.

So the contract is **machine-readable but not machine-retrievable**. It exists, it is complete, and
Splunk simply does not offer it as a file. Every specification in this repository carries
`info.x-apievangelist-derivation` and `info.x-apievangelist-source` naming the page it came from.

## What is here

| Surface | Finding |
|---|---|
| REST | 48 OpenAPI documents, 242 operations, reconstructed from Splunk's own reference payload |
| Streaming | SignalFlow WebSocket + Server-Sent Events; AsyncAPI 3.0.0 derived in `asyncapi/` |
| Webhooks | Outbound alert notifications signed with `X-SFX-Signature` (HMAC-SHA256, base64) |
| MCP | Hosted server, 12 tools, probed live — HTTP 401 unauthenticated, so the server is real and the schema is gated |
| A2A | No agent card on any host. Nothing was written. |
| Auth | One `X-SF-TOKEN` header, two token classes. No OAuth, no OIDC, no scopes. |
| Idempotency | **None.** No idempotency key anywhere; a retried POST creates a second real object. |
| Rate limits | Two opt-in per-org-token limits. **No rate-limit response headers at all** — 429 arrives with no budget signal. |
| Errors | Not RFC 9457. Five vendor JSON envelopes plus a bare-string form. |
| Sandbox | **None.** No test mode, no test values. Every token is a live token. |

## Verified links

- [Developer portal](https://dev.splunk.com/observability/)
- [API reference](https://dev.splunk.com/observability/reference/)
- [Endpoint overview](https://dev.splunk.com/observability/docs/apibasics/api_list/)
- [Authentication](https://dev.splunk.com/observability/docs/apibasics/authentication_basics/)
- [MCP server](https://help.splunk.com/en/splunk-observability-cloud/splunk-ai-assistant/interact-with-your-observability-data-using-the-splunk-mcp-server)
- [Pricing](https://www.splunk.com/en_us/products/pricing/observability.html)
- [Release notes](https://help.splunk.com/en/splunk-observability-cloud/release-notes)
- [Status](https://status.signalfx.com)
- [Trust center](https://customertrust.splunk.com/)
- [Security policy](https://advisory.splunk.com/report)
- [ParentCompany](https://apis.io/providers/splunk/)
- [GitHubOrganization](https://github.com/splunk)
- [GitHubOrganization](https://github.com/signalfx)

All URLs above returned HTTP 200 when probed on 2026-08-19.
