---
name: Triage Splunk Observability Cloud incidents and mute noisy alerts
description: List open incidents, inspect one, clear it, and create a time-boxed muting rule.
api: openapi/splunk-observability-incidents-openapi.yml
operations:
  - Retrieve Incidents
  - Retrieve Incident ID
  - Clear Single Incident
  - Clear Incidents
  - Create Single Muting Rule
  - Retrieve Muting Rules Using Query
  - Unmute Single Muting Rule
generated: '2026-08-19'
method: generated
source: openapi/splunk-observability-incidents-openapi.yml, openapi/splunk-observability-detectors-openapi.yml
---

# Triage incidents and mute noisy alerts

## Steps

1. **List what is open.** `GET /incident` (`Retrieve Incidents`). Scope to one detector instead
   with `GET /detector/{id}/incidents` (`Retrieve Incidents Single Detector`) — Splunk's own docs
   recommend the detector-scoped path because it matches a much smaller set.
2. **Inspect.** `GET /incident/{id}` (`Retrieve Incident ID`).
3. **Clear.** `PUT /incident/{id}/clear` (`Clear Single Incident`) for one, or
   `PUT /incident/clear` (`Clear Incidents`) for a batch. Clearing is a state change on a live
   organization and is not reversible through the API.
4. **Mute rather than delete.** `POST /alertmuting` (`Create Single Muting Rule`) with a filter and
   a start/stop window. Muting suppresses notifications without touching the detector, which is the
   right move during a known maintenance window.
5. **Review muting.** `GET /alertmuting` (`Retrieve Muting Rules Using Query`).
6. **End it early.** `PUT /alertmuting/{id}/unmute` (`Unmute Single Muting Rule`).

## Event history

`GET /event/find` (`Retrieve Events Using Query`, Retrieve Events V2) searches detector events and
custom events together, and returns **at most 10,000** events. For detector work prefer
`GET /detector/{id}/events` (`Retrieve Events Single Detector`).

## Errors

- **404** on an incident ID usually means wrong realm or wrong object type — IDs carry no type
  prefix, so a detector ID passed as an incident ID looks identical and simply misses.
- **429** on `GET /v1/event` is the **event search limit**, configurable per org token between 1 and
  30 requests per minute. No headers accompany it.
