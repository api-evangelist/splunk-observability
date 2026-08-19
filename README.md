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

Splunk Observability Cloud is the observability platform Splunk built on SignalFx, now part of Cisco: infrastructure monitoring, APM, real user monitoring, synthetics, log observer and incident response over OpenTelemetry-native ingest. It is the largest documented API surface Splunk operates — the developer site enumerates forty-eight distinct OpenAPI specifications covering charts, dashboards, detectors, SignalFlow, SLOs, org tokens, teams and twenty synthetics endpoints. None of them is fetchable: dev.splunk.com answers HTTP 200 with an identical 6,638-byte single-page-application shell for every path, including invented control paths, so the entire contract surface is readable by people and invisible to machines.

## Ownership

Part of the Splunk family.

## Contract status

The vendor's developer host is a soft-404 farm: every path returns HTTP 200 with an identical SPA shell, including invented control paths. No contract can be confirmed by a machine. API Evangelist has not authored a substitute.

## Verified links

- [Portal](https://dev.splunk.com/observability/reference/)
- [Documentation](https://dev.splunk.com/observability/reference/)
- [APIReference](https://dev.splunk.com/observability/docs/apibasics/api_list/)
- [ParentCompany](https://apis.io/providers/splunk/)
- [GitHubOrganization](https://github.com/splunk)
- [GitHubOrganization](https://github.com/CiscoDevNet)

All URLs above returned HTTP 200 when probed on 2026-08-19.
