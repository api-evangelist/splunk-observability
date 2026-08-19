---
name: Create a Splunk Observability Cloud detector and route its alerts
description: Validate a SignalFlow detector, create it, wire a signed webhook notification, and confirm the incident path works.
api: openapi/splunk-observability-detectors-openapi.yml
operations:
  - Validate Detector Definition
  - Create Single Detector
  - Retrieve Detector ID
  - Enable Detect Blocks
  - Create Integration
  - Validate Integration
  - Retrieve Incidents Single Detector
generated: '2026-08-19'
method: generated
source: openapi/splunk-observability-detectors-openapi.yml, openapi/splunk-observability-integrations-openapi.yml, conventions/, errors/
---

# Create a detector and route its alerts

## Before you start

- Base URL is `https://api.<REALM>.observability.splunkcloud.com/v2`. **The realm is part of the
  hostname.** Getting it wrong produces a 404, not a redirect.
- Every request carries `X-SF-TOKEN`. Creating or updating a detector requires the **admin** or
  **power** role; a `read_only` token gets 403.
- **There is no idempotency key.** `POST /detector` is not safe to blind-retry — a retry creates a
  second detector. If a create times out, search with `Retrieve Detectors Query` by name before
  retrying.

## Steps

1. **Validate the program first.** `POST /detector/validate` (`Validate Detector Definition`) with
   the detector body. This is the cheap way to find SignalFlow errors; a 400 here returns the
   validation message rather than leaving a half-built object.
2. **Create the detector.** `POST /detector` (`Create Single Detector`). Keep the returned `id`.
   It is an opaque 11-character string with no type prefix — record what kind of object it is,
   because you cannot tell later by looking at it.
3. **Confirm.** `GET /detector/{id}` (`Retrieve Detector ID`).
4. **Create the notification target.** `POST /integration` (`Create Integration`) with a webhook
   integration: set `url`, and set `sharedSecret` so Splunk signs the payload. With `sharedSecret`
   present, Splunk adds `X-SFX-Signature` — a base64-encoded HMAC-SHA256 of the raw body keyed on
   the secret. Verify it on receipt; do not put the header in `headers` yourself.
5. **Validate the integration.** `GET /integration/validate/{id}` (`Validate Integration`).
6. **Enable the detect blocks** if they were created disabled: `PUT /detector/{id}/enable`
   (`Enable Detect Blocks`).
7. **Check the incident path.** `GET /detector/{id}/incidents`
   (`Retrieve Incidents Single Detector`) and `GET /detector/{id}/events`
   (`Retrieve Events Single Detector`).

## Errors

- **400** — the SignalFlow program or the rule body failed validation. Read `message`; several
  endpoints enumerate the exact strings.
- **403** — token role is too low for a write. Reads accept `read_only`; writes need admin or power.
- **429** — an org-token rate limit was hit. **No rate-limit headers are returned**, so back off
  exponentially; there is no budget to read.
- Error envelopes are not uniform. Detector endpoints return a bare JSON string for some 4xx
  statuses and `{code, message}` for others. Parse defensively — see
  `errors/splunk-observability-problem-types.yml`.

## Clean up

`DELETE /detector/{id}` (`Delete Single Detector`) and `DELETE /integration/{id}`
(`Delete Single Integration`). There is no sandbox: everything you create here is a live object in
a real organization.
