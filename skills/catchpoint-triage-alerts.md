---
name: Triage Catchpoint alerts
description: Retrieve and inspect Catchpoint test alerts, drill into individual alerts with node detail, and route them via webhooks.
api: openapi/catchpoint-rest-api-v2-openapi-original.json
generated: '2026-07-18'
method: generated
operations:
  - GET /v2/tests/alerts
  - GET /v2/tests/alerts/{alertIds}
  - GET /v2/tests/alert/state/{testIds}
---

# Triage Catchpoint alerts

Query and inspect alert activity from Catchpoint REST API v2
(`https://io.catchpoint.com/api`). Authenticate with the REST API key as a bearer
token. Responses are JSON; errors use `{ "errors": [ { "message": "..." } ] }`.

## Steps

1. **List alerts in a window.** `GET /v2/tests/alerts` filtered by `testIds`,
   time range (UTC `YYYY-MM-DDTHH:MI:SS`), and `includeNodeDetails=true` when you
   need per-node breakdown. Page with `pageNumber` / `pageSize`; keep paging while
   the response `hasMore` is `true`.
2. **Inspect a specific alert.** `GET /v2/tests/alerts/{alertIds}` to pull the full
   alert record (alertType, trigger thresholds, node).
3. **Check current alert state per test.** `GET /v2/tests/alert/state/{testIds}`.

## Notes

- When a test's alert condition is "Average Across Nodes", node details are not
  returned and `includeNodeDetails=true` yields an error — do not treat that as a
  failure of the alert.
- For push delivery instead of polling, configure an **Alert Webhook** (JSON/XML,
  macro-templated, signable) — see `asyncapi/catchpoint-webhooks.yml`.
- HTTP 429 means a rate limit was hit; back off and retry.
