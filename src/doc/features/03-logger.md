# observability/logger.js

**nginx equivalent:** `access_log` / `error_log` with `log_format`

**Purpose:** Structured, leveled logging used by every other module. Depends only on `config.js`.

## Tier 1 — Core (required)

- [ ] Log levels: at minimum `info`, `warn`, `error`, `debug` — `debug` gated behind a config flag so it's silent by default.
- [ ] A standard **request log line** per request: method, path, status code, duration in ms (this is the line every module's tests will assert against, keep the format stable).
- [ ] Timestamps on every line (ISO 8601, matches what you already have in your run logs).

## Tier 2 — Production-grade (if time allows)

- [ ] Configurable log format (like nginx's `log_format` directive) — pick a couple of named presets (`combined`, `short`) rather than a full templating language.
- [ ] Log rotation by size or daily rollover, keeping N historical files.
- [ ] Dual targets: stdout **and** a file simultaneously, independently toggleable.

## Tier 3 — Out of scope (name it, don't chase it)

- Syslog / remote log shipping.
- Full structured JSON logs with request correlation IDs threaded through every downstream log line.
