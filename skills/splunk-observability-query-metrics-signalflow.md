---
name: Find metrics and run a SignalFlow computation
description: Discover metric names and dimensions, validate a program, execute it, and consume the streaming result.
api: openapi/splunk-observability-signalflow-openapi.yml
operations:
  - Retrieve Metadata MetricsQuery
  - Retrieve Metadata Metric Name
  - Retrieve Dimensions Query
  - Retrieve Metric Timeseries Metadata
  - Execute SignalFlow computation
  - Retrieve Computation Feedback
  - Stop SignalFlow Computation
  - Create WebSocket Connection
generated: '2026-08-19'
method: generated
source: openapi/splunk-observability-metrics-metadata-openapi.yml, openapi/splunk-observability-signalflow-openapi.yml, asyncapi/splunk-observability-signalflow-asyncapi.yml
---

# Find metrics and run a SignalFlow computation

## Two different hosts

Metadata lives on `https://api.<REALM>.observability.splunkcloud.com/v2`. SignalFlow lives on
`https://stream.<REALM>.observability.splunkcloud.com/v2/signalflow`. Do not send SignalFlow calls
to the api host.

## Steps

1. **Find the metric.** `GET /metric` (`Retrieve Metadata MetricsQuery`) with a query; then
   `GET /metric/{name}` (`Retrieve Metadata Metric Name`) for detail.
2. **Find the dimensions.** `GET /dimension` (`Retrieve Dimensions Query`) and
   `GET /metrictimeseries` (`Retrieve Metric Timeseries Metadata`) to learn which MTS exist and
   what filters will actually match.
3. **Run it.** `POST /execute` (`Execute SignalFlow computation`) on the stream host with the
   program text. The response arrives as **Server-Sent Events**, `content-type: text/event-stream`.
4. **Or stream it.** `GET /connect` (`Create WebSocket Connection`), then send
   `{"type":"authenticate","token":"..."}` **within 5 seconds**, then
   `{"type":"execute","channel":"channel-1","program":"..."}`. One connection multiplexes many
   channels. See `asyncapi/splunk-observability-signalflow-asyncapi.yml` for the full message set.
5. **Watch progress.** `GET /{id}/feedback` (`Retrieve Computation Feedback`).
6. **Always stop it.** `POST /{id}/stop` (`Stop SignalFlow Computation`). A running computation
   consumes job capacity until it is stopped.

## Reading the stream

- `control-message` carries `event`: `STREAM_START`, `JOB_START` (with `handle`), `JOB_PROGRESS`
  (with `progress`), `CHANNEL_ABORT`, `END_OF_CHANNEL`.
- `metadata` arrives **before** the data for a `tsId`. Buffer metadata; you will need it to label
  points.
- `data` messages are **binary over WebSocket** — 4-byte preamble, 16-byte channel name, int64
  logical timestamp, int32 element count, then 17-byte tuples of value-type / tsId / value, all
  big-endian. Over SSE the same message is JSON.
- `event` messages fire when a `detect()` block trips or clears, carrying `incidentId`, `was` and
  `is`.

## Rate limits

`POST /v2/signalflow/execute` is the one endpoint with a named, configurable rate limit: the
**SignalFlow job start limit**, settable per org token between 1 and 60 requests per minute.
Exceeding it returns **429 with no rate-limit headers**. If you are starting jobs in a loop, pace
yourself deliberately — the API will not tell you how much budget is left.
