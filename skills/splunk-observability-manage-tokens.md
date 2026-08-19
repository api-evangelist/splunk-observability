---
name: Manage Splunk Observability Cloud authentication tokens
description: Create a session token, create and scope an org token, rotate its secret, and retire it.
api: openapi/splunk-observability-org-tokens-openapi.yml
operations:
  - Create Session Token
  - Delete Session Token
  - Create Single Token
  - Retrieve Tokens Using Query
  - Retrieve Token Using Name
  - Update Single Token
  - Rotate Token Secret
  - Delete Single Token
generated: '2026-08-19'
method: generated
source: openapi/splunk-observability-sessiontokens-openapi.yml, openapi/splunk-observability-org-tokens-openapi.yml, https://dev.splunk.com/observability/docs/apibasics/authentication_basics/
---

# Manage authentication tokens

## The two token classes

| Class | UI name | Lifetime | Scope | Created by |
|---|---|---|---|---|
| Session token | User API Access Token | short-lived | one user | `POST /v2/session` |
| Org token | Access Token | long-lived | whole organization | `POST /v2/token` |

Both travel in the same `X-SF-TOKEN` header, so a receiving system cannot tell them apart from the
request alone. Track which one you hold.

## Steps

1. **Get a session token without a token.** `POST /session` (`Create Session Token`) takes the
   `email` and `password` of an organization member — this is the only unauthenticated operation in
   the API. Treat it accordingly.
2. **Create an org token.** `POST /token` (`Create Single Token`). Org tokens carry permissions
   (for example RUM and Ingest) and can carry **usage limits**, including the two rate-related
   limits: the SignalFlow job start limit (1–60/min) and the event search limit (1–30/min).
   Setting a limit to 0 removes it.
3. **Audit.** `GET /token` (`Retrieve Tokens Using Query`) and `GET /token/{name}`
   (`Retrieve Token Using Name`). Note that org tokens are addressed **by name**, not by ID.
4. **Rotate.** `POST /token/{name}/rotate` (`Rotate Token Secret`) — the correct response to a
   suspected leak, because it preserves the token's identity and limits while changing the secret.
5. **Retire.** `DELETE /token/{name}` (`Delete Single Token`), and
   `DELETE /session` (`Delete Session Token`) to invalidate a session token.

## Agent guidance

- Prefer an **org token with the narrowest permission set and an explicit rate limit** for anything
  automated. A rate-limited token is the only way to bound an agent's request volume, because the
  API returns no rate-limit headers for the agent to self-pace on.
- Never log the secret. Splunk returns it once at creation and at rotation.
