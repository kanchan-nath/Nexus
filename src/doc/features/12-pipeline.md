# core/pipeline.js

**nginx equivalent:** nginx's request-processing phase engine (the internal sequence every request walks through: rewrite → access → content → log)

**Purpose:** Wire every module above into the actual per-request handler. This is the highest-risk file for ordering bugs — the reference build's dashboard bug came directly from getting this order wrong.

## Tier 1 — Core (required)

- [ ] **Correct phase order**, in this exact sequence: static/dashboard routes (served directly, never proxied) → rate limit check → route match → auth check → backend selection → forward to backend.
- [ ] Auth check must come **after** route match, not before — an unmatched path returns 404, never a 401 for a route that doesn't even exist.
- [ ] `metrics.recordRequest()` called on literally every terminal branch (429, 404, 401, 502, and the final success/error on `res.on('finish')`) — no early return should skip it.
- [ ] WAL append/update wired around the actual forward call, not around branches that never reach a backend.

## Tier 2 — Production-grade (if time allows)

- [ ] Pull the phase order itself into config as an ordered list of named steps, so the sequence is declared, not hardcoded — makes the ordering bug class structurally harder to reintroduce.
- [ ] Per-request context object threaded through every phase (instead of the current pattern of re-deriving `clientIp`, `parsedUrl`, etc. inline) — easier to unit test each phase in isolation.

## Tier 3 — Out of scope (name it, don't chase it)

- A true pluggable middleware/module system with third-party hook registration (nginx's actual module API) — full generality isn't needed for a single gateway build.
