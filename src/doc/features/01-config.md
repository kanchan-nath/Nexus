# config.js

**nginx equivalent:** `nginx.conf` directive parsing

**Purpose:** Load, parse, and validate `nexus.config.json` into the shape every other module depends on. This is the first file every other module imports — get its shape right before writing anything downstream.

## Tier 1 — Core (required)

- [ ] Load and `JSON.parse` the config file from a `--config` path (or default `./nexus.config.json`).
- [ ] Validate required top-level keys exist: `listen`, `backends`, `routes` (or equivalent) — fail fast with a clear error message naming the missing key, not a generic crash.
- [ ] Apply sane defaults for optional fields (e.g. health check interval, rate limit window) so every downstream module can assume they exist.
- [ ] Export a single typed/validated config object — no module downstream should need to re-check `if (config.foo)` for required fields.

## Tier 2 — Production-grade (if time allows)

- [ ] Config **hot-reload**: watch the file or listen for `SIGHUP`, re-parse, validate the new version, and atomically swap it in without dropping in-flight connections.
- [ ] Reject an invalid reload (bad JSON, missing required field) by logging the error and **keeping the last-known-good config running** — never let a bad reload take down a healthy process.
- [ ] A `--dry-run` / `-t` style validate-only mode (parse + validate, print OK/errors, exit — no server start).

## Tier 3 — Out of scope (name it, don't chase it)

- A full custom config language with directives/includes (nginx's actual `nginx.conf` grammar) — JSON is the right call for a 72-hour build, don't build a parser for this.
- Config templating / variable substitution across environments.
