---
name: Provision a Catchpoint synthetic test
description: Create and verify a synthetic monitoring test in Catchpoint — resolve the target folder and monitoring nodes, POST the test, then confirm it.
api: openapi/catchpoint-rest-api-v2-openapi-original.json
generated: '2026-07-18'
method: generated
operations:
  - GET /v2/nodes
  - GET /v2/nodes/groups
  - GET /v2/folders
  - POST /v2/tests
  - GET /v2/tests/{testIds}
---

# Provision a Catchpoint synthetic test

Use Catchpoint REST API v2 (`https://io.catchpoint.com/api`) to stand up a synthetic
test. Every request sends the REST API key as a bearer token:
`Authorization: Bearer <REST_API_KEY>` (create the key in Portal → Settings →
Integrations → REST API). Responses are JSON; errors arrive as
`{ "errors": [ { "message": "..." } ] }`.

## Steps

1. **Pick monitoring vantage points.** `GET /v2/nodes` (or `GET /v2/nodes/groups`
   for a named group) to resolve the node IDs / node-group IDs you will target.
2. **Resolve the destination folder.** `GET /v2/folders` to find the `folderId`
   under the right product where the test should live (Division → Product → Folder →
   Test hierarchy; settings inherit down the tree).
3. **Create the test.** `POST /v2/tests` with the JSON payload describing the test
   (name, test type per REST API enumeration — e.g. Web=0, API=9, DNS=5 —, target
   URL, folder, and the node/node-group targeting from step 1). A duplicate test name
   returns HTTP 400.
4. **Confirm.** `GET /v2/tests/{testIds}` with the returned id to verify configuration.

## Rules

- Parameters are case sensitive; multiple query params combine with AND.
- Dates are UTC `YYYY-MM-DDTHH:MI:SS`.
- Respect rate limits (default 7/sec, 20/min, 500/hour, 2000/day) — HTTP 429 on excess; back off.
- Use `PATCH /v2/tests/{testId}` (add/remove/replace ops) for later edits; required properties cannot be removed.
