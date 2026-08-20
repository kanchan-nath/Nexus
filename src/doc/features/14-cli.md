# cli.js

**nginx equivalent:** the `nginx` binary's own flags (`-s reload`, `-s stop`, `-t`)

**Purpose:** Parse argv, load config, and start the server. The actual process entrypoint.

## Tier 1 — Core (required)

- [ ] `start` command accepting `--config <path>` (default to `./nexus.config.json` if omitted).
- [ ] Clear, non-stack-trace error output on bad/missing config path or invalid JSON — this is the first thing a judge sees if they mistype a flag.
- [ ] Wires `SIGINT`/`SIGTERM` to `shutdownServer()` for a clean Ctrl+C.

## Tier 2 — Production-grade (if time allows)

- [ ] `-t` / `--test` flag: validate config only, print OK or the specific validation error, exit without starting a server (mirrors nginx's own `-t` flag almost exactly — a nice, cheap thing to point at during Q&A).
- [ ] `-s reload` equivalent: send a reload signal to a running instance to hot-swap config (pairs with the Tier 2 hot-reload work in `config.js`).

## Tier 3 — Out of scope (name it, don't chase it)

- A full subcommand suite (`status`, `logs`, `restart`, etc.) — scope creep for a single gateway binary, not worth it.
