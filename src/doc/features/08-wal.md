# reliability/wal.js

**nginx equivalent:** *(no direct nginx equivalent — this is a Nexus differentiator, call it out as such)*

**Purpose:** Append-only durability log of requests/responses. Depends on `config.js` and `logger.js`.

## Tier 1 — Core (required)

- [ ] Append-only write on request start, update on response finish — batched writes with a configurable flush interval (you already have `batch`/`flush` config values, keep them).
- [ ] Configurable max file size with rollover to a new file, keeping N historical files.
- [ ] Clean `stop()` that flushes any buffered entries before process exit — this is what your `shutdownServer()` already calls, keep that contract.

## Tier 2 — Production-grade (if time allows)

- [ ] A replay/recovery read path: on startup, read the WAL and report the last N entries or detect an unclean shutdown.
- [ ] Rotation cleanup: actually delete files beyond the configured retention count (confirm this is implemented, not just configured).

## Tier 3 — Out of scope (name it, don't chase it)

- Log compaction.
- Crash-consistent replay that reconstructs in-flight request state after a hard crash.
