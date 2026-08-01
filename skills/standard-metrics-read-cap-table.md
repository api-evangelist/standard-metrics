---
name: Read a company cap table
description: Retrieve cap-table data — financing rounds, securities, share classes, and transactions — for portfolio companies from the Standard Metrics Investment Data API.
api: openapi/standard-metrics-investment-data-openapi.json
operations: [get_financing_events, get_securities, get_share_classes, get_transactions]
---

# Read a company cap table

Use this to assemble a company's cap table from the Investment Data API (beta).

## Auth
Get a Bearer token from `https://api.standardmetrics.io/o/token/` (client_credentials) and send `Authorization: Bearer <token>`. Base host: `https://app.standardmetrics.io`; paths are under `/beta/investment/`.

## Steps
1. **Financing events** — `get_financing_events` (`POST /beta/investment/financing-events/get/`) for rounds: stage, pre-money valuation, round size, closing date, co-investors. Filter by `company_ids`; optionally `is_crypto`, `co_investors`, `sort_by`.
2. **Share classes** — `get_share_classes` (`POST /beta/investment/share-classes/get/`) for pricing/type; filter by `company_ids`, `purchase_type` (primary/secondary).
3. **Securities** — `get_securities` (`POST /beta/investment/securities/get/`) for certificates, warrants, convertibles, token securities; filter by `company_ids`, `fund_id`.
4. **Transactions** — `get_transactions` (`POST /beta/investment/transactions/get/`) for sales, conversions, exercises; filter by `event`, `input_security_id`, `output_security_id`.

## Rules
- These are POST-with-body "get" endpoints — pass filters in the JSON body, not query params.
- Set `include_fx_details: true` to get original currency, value, and exchange rate on money fields.
- This is a beta surface; watch the changelog for field additions (responses now include denormalized `company_name`, `fund_name`, `share_class_name`).
- Respect the 30 req / 10 s per-token rate limit; back off on HTTP 429.
