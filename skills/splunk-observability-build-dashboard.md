---
name: Build a Splunk Observability Cloud dashboard from charts
description: Create a dashboard group, create charts from SignalFlow programs, assemble a dashboard, and find it again.
api: openapi/splunk-observability-dashboards-openapi.yml
operations:
  - Create Single Dashboard Group
  - Create Single Chart
  - Create Single Dashboard
  - Create Simple Dashboard
  - Clone Dashboard to Group
  - Retrieve Dashboards Using Query
generated: '2026-08-19'
method: generated
source: openapi/splunk-observability-charts-openapi.yml, openapi/splunk-observability-dashboards-openapi.yml, openapi/splunk-observability-dashboard-groups-openapi.yml
---

# Build a dashboard from charts

## Before you start

- Base URL `https://api.<REALM>.observability.splunkcloud.com/v2`, header `X-SF-TOKEN`.
- Writes need the **admin** or **power** role; `GET /chart` and `GET /chart/{id}` also accept
  `read_only`.
- A chart is a SignalFlow program plus display options. The program goes in `programText`.
- `tags` is capped at **50 items** per chart.

## Steps

1. **Create the group.** `POST /dashboardgroup` (`Create Single Dashboard Group`). Dashboards
   belong to groups; create the container first so you never orphan a dashboard.
2. **Create each chart.** `POST /chart` (`Create Single Chart`) with `name`, `programText` and
   `options`. Keep every returned `id`.
3. **Assemble the dashboard.** `POST /dashboard` (`Create Single Dashboard`) referencing the chart
   IDs, or take the shortcut: `POST /dashboard/simple` (`Create Simple Dashboard`) creates a
   dashboard *and* its charts in one call.
4. **Place it.** `POST /dashboardgroup/{id}/dashboard` (`Clone Dashboard to Group`) to put a copy
   into another group.
5. **Find it later.** `GET /dashboard` (`Retrieve Dashboards Using Query`) with `name`, `tags`,
   `limit` and `offset`.

## Paging

`limit` defaults to 50 and `offset` is 0-indexed. Repeating `tags` ORs the values
(`tags=cpu&tags=prod`). **No query returns more than 10,000 objects** no matter how you page —
filter by name or tag rather than walking the whole collection.

## Retries

There is no idempotency key. If `POST /chart` or `POST /dashboard` times out, search by name before
retrying — otherwise you will create a duplicate. `PUT /{id}` is naturally idempotent; prefer it.
