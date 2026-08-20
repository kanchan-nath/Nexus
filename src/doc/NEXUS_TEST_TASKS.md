# Nexus — Test Writing Task Distribution

**Team:** Kanchan, Ashish, Saikat, Biyas
**Rule:** When you finish a task, change `[ ]` to `[x]` and add your name + date next to it.
Example: `- [x] security/tls.js → test/security/tls.test.js` — done by Ashish (Aug 21)

---

## Task List (file creation / dependency order)

| # | Source file | Test file | Assigned to | Done |
|---|---|---|---|---|
| 1 | `config.js` | `test/config.test.js` | **Saikat** | [ ] |
| 2 | `security/tls.js` | `test/security/tls.test.js` | **Ashish** | [ ] |
| 3 | `observability/logger.js` | `test/observability/logger.test.js` | **Saikat** | [ ] |
| 4 | `observability/metrics.js` | `test/observability/metrics.test.js` | **Saikat** | [ ] |
| 5 | `routing/router.js` | `test/routing/router.test.js` | **Kanchan** | [ ] |
| 6 | `routing/loadbalancer.js` | `test/routing/loadbalancer.test.js` | **Kanchan** | [ ] |
| 7 | `reliability/healthcheck.js` | `test/reliability/healthcheck.test.js` | **Ashish** | [ ] |
| 8 | `reliability/wal.js` | `test/reliability/wal.test.js` | **Ashish** | [ ] |
| 9 | `security/ratelimiter.js` | `test/security/ratelimiter.test.js` | **Ashish** | [ ] |
| 10 | `security/auth.js` | `test/security/auth.test.js` | **Ashish** | [ ] |
| 11 | `observability/dashboard.js` | `test/observability/dashboard.test.js` | **Biyas** | [ ] |
| 12 | `core/pipeline.js` | `test/core/pipeline.test.js` *(integration — mock/stub #2–11)* | **Kanchan** | [ ] |
| 13 | `core/server.js` | `test/core/server.test.js` *(real server, hit via `http.request`)* | **Kanchan** | [ ] |
| 14 | `cli.js` | `test/cli.test.js` *(argv parsing, config load path)* | **Saikat** | [ ] |
| 15 | `examples/backend-echo.js` | *skip — throwaway demo fixture, just confirm it runs* | **Biyas** | [ ] |
| 16 | `scripts/start.js` | *skip — orchestrator script, one-time smoke test* | **Biyas** | [ ] |
| 17 | `public/index.html` | *no test — verify manually against dashboard SSE stream* | **Biyas** | [ ] |

**How to mark done:** flip `[ ]` to `[x]` in both the row's Done column, e.g. `| [x] Ashish (Aug 21) |`.

**Notes on order:**
- Items 1–11 are independent modules — can be done in parallel, any order within that block.
- Item 12 (`pipeline.js`) needs 2–11 at least stubbed out — don't start early.
- Item 13 (`server.js`) needs router + pipeline stable — do after 12.
- Item 14 (`cli.js`) needs `config.js` (item 1) done first.
- Items 15–17 have no hard dependency — Biyas can do these anytime, ideally close to demo day since 17 is what judges actually see live.
