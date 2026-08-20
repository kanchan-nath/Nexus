# observability/dashboard.js

**nginx equivalent:** `stub_status` (Nexus's live SSE dashboard is meaningfully richer than nginx's OSS equivalent)

**Purpose:** Serve the live-updating dashboard UI's data feed. Depends on `config.js` and `metrics.js`.

## Tier 1 — Core (required)

- [ ] SSE (`text/event-stream`) endpoint that pushes a metrics snapshot on an interval.
- [ ] Snapshot includes: totals, per-route breakdown, per-backend breakdown — matching exactly what `public/index.html` renders.
- [ ] Never counted in its own metrics (the dashboard's own SSE connection shouldn't pollute `TOTAL REQUESTS`).

## Tier 2 — Production-grade (if time allows)

- [ ] Live diffing (only push changed fields) instead of a full snapshot every interval, once request volume is high enough for it to matter.
- [ ] A REST snapshot endpoint (`GET /nexus/metrics`) alongside SSE, for tooling that can't consume a stream.

## Tier 3 — Out of scope (name it, don't chase it)

- Historical time-series persistence (metrics surviving a restart, queryable over a time range) — this starts to become a self-built Grafana, genuinely out of scope.
