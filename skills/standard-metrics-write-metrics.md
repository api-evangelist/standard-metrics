---
name: Write metrics for a company
description: Upload standard or custom metric data for a portfolio company, resolving the company and metric category first and handling conflicts.
api: openapi/standard-metrics-main-openapi.json
operations: [companies_list, metrics_options_list, metrics_create]
---

# Write metrics for a company

Use this to push metric datapoints (standard, custom, or budget) into Standard Metrics.

## Auth
Get a Bearer token from `https://api.standardmetrics.io/o/token/` (client_credentials) and send `Authorization: Bearer <token>`. Requires a `write` scope and EDITOR/ADMIN role.

## Steps
1. **Resolve the company** — `companies_list` (`GET /companies/`) to get the target company `id` (or `companies_create` to create an offline company first).
2. **Resolve the metric category** — `metrics_options_list` (`GET /metrics/options/`) to confirm the category name/type/cadence and whether it is a select-style metric with predefined options.
3. **Create metrics** — `metrics_create` (`POST /metrics/`). Supports standard, custom, select-style, and budget metrics.

## Rules
- Duplicate metrics in a single payload are rejected with `400` identifying the conflict by `category`, `date`, `metric_cadence`, `budget_id`.
- Control conflicts with pre-existing metrics via the `on_conflict` query parameter (`skip` or `overwrite`).
- Use only supported cadences (point_in_time, month, quarter, year); day/week/half_year are deprecated.
- No idempotency-key contract exists — do not retry a `POST /metrics/` blindly; check the response and use `on_conflict`.
