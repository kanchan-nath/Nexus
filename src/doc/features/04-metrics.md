# observability/metrics.js

**nginx equivalent:** `stub_status` / third-party `vts` module

**Purpose:** In-memory counters and timers that back the dashboard. Independent of `logger.js` — don't let one depend on the other.

## Tier 1 — Core (required)

- [ ] `recordRequest()` called on **every single response path** — including 404s, 401s, 429s, and 502s, not just successful proxied requests. (This was the actual bug that made the dashboard show `100% error rate` from two stray `/favicon.ico` 401s — audit every early-return branch in the pipeline to confirm it calls this.)
- [ ] Totals: total requests, total errors, error rate.
- [ ] Per-route and per-backend breakdown (request count, error count, avg latency) — this is what powers the dashboard's `BACKENDS` / `ROUTES` tables.
- [ ] Rolling-window average latency.

## Tier 2 — Production-grade (if time allows)

- [ ] Percentile latencies — p50/p95/p99, not just average (averages hide tail latency, which is usually the more interesting number).
- [ ] Configurable rolling window size (currently hardcoded — make it a config value).

## Tier 3 — Out of scope (name it, don't chase it)

- Prometheus-format `/metrics` export (text exposition format) for real scraping — a genuinely valuable add if you have spare hours, since it's a well-known format judges may recognize instantly.
