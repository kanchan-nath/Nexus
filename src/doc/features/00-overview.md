# Nexus Feature Roadmap — nginx Parity Map

Each module has its own doc (`docs/<module>.md`) with a three-tier feature checklist:

- **Tier 1 — Core:** required to honestly call the module "done." Judge-visible, directly maps to a named nginx feature.
- **Tier 2 — Production-grade:** what separates a demo from something you'd trust in front of real traffic. Do these if time allows.
- **Tier 3 — Out of scope:** genuine nginx capabilities, deliberately not attempted in a 72-hour build. List these explicitly in your README as "acknowledged, not attempted" — naming a gap on purpose reads stronger than silently missing it.

## Module index (write order = dependency order, see conversation history)

| # | Module | nginx equivalent |
|---|--------|-------------------|
| 1 | `config.js` | `nginx.conf` parsing |
| 2 | `security/tls.js` | `ssl_certificate` / TLS termination |
| 3 | `observability/logger.js` | `access_log` / `error_log` |
| 4 | `observability/metrics.js` | `stub_status` module |
| 5 | `routing/router.js` | `location` blocks / `server_name` |
| 6 | `routing/loadbalancer.js` | `upstream` block |
| 7 | `reliability/healthcheck.js` | active/passive health checks |
| 8 | `reliability/wal.js` | *(no native nginx equivalent — Nexus differentiator)* |
| 9 | `security/ratelimiter.js` | `limit_req_zone` |
| 10 | `security/auth.js` | `auth_basic` / `auth_request` |
| 11 | `observability/dashboard.js` | `stub_status` (Nexus's is richer) |
| 12 | `core/pipeline.js` | nginx's request-processing phase engine |
| 13 | `core/server.js` | master listener sockets |
| 14 | `cli.js` | the `nginx` binary's flags (`-s reload`, `-t`) |

Not covered by individual docs (support files, not core proxy logic):
- `examples/backend-echo.js` — demo fixture only
- `scripts/start.js` — dev/demo orchestrator only
- `public/index.html` — static asset, follows `dashboard.js`'s SSE contract

## How to use these docs while building

1. Open the doc for the module you're about to write.
2. Implement everything in **Tier 1** — that's the acceptance bar for "module done."
3. If time remains after all 14 modules hit Tier 1, sweep back through and pick up Tier 2 items, prioritizing whichever gives the strongest demo/judge-visible impact (health check recovery thresholds and keep-alive upstream connections are usually the highest ROI for effort spent).
4. Never attempt Tier 3 mid-hackathon — it's listed so you can name it confidently if asked, not so you chase it.
