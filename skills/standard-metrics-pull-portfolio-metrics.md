---
name: Pull portfolio company metrics
description: Authenticate to the Standard Metrics API, list portfolio companies, discover available metric categories, and pull time-series metrics for one or more companies.
api: openapi/standard-metrics-main-openapi.json
operations: [companies_list, metrics_options_list, metrics_list]
---

# Pull portfolio company metrics

Use this to retrieve financial metrics for a firm's portfolio companies.

## Auth
1. Obtain a Bearer token: POST `client_id`/`client_secret` (Basic auth) to `https://api.standardmetrics.io/o/token/` with `grant_type=client_credentials`. The token (`access_token`) expires in 3600s.
2. Send `Authorization: Bearer <access_token>` on every request. Base URL: `https://api.standardmetrics.io/v1`.

## Steps
1. **List companies** — `companies_list` (`GET /companies/`). Page through with `page`/`per_page` (max 100). Capture each company `id`.
2. **Discover metric options** — `metrics_options_list` (`GET /metrics/options/`) to learn the available category names, types, and cadences before filtering.
3. **Get metrics** — `metrics_list` (`GET /metrics/`) filtered by company, `category`, date range, and `cadence` (point_in_time, month, quarter, year — avoid the deprecated day/week/half_year cadences).

## Rules
- All list endpoints are paginated (max 100/page); loop until the page is short.
- Respect rate limits: 30 requests / 10 seconds per token (and 500/min). On HTTP 429, back off exponentially.
- Errors are plain HTTP status codes (401 auth, 403 permission, 422 validation); see errors/standard-metrics-problem-types.yml.
