# security/ratelimiter.js

**nginx equivalent:** `limit_req_zone`

**Purpose:** Per-IP request throttling. Depends only on `config.js`.

## Tier 1 — Core (required)

- [ ] Token bucket (or leaky bucket) per client IP.
- [ ] Return a clean allowed/denied result the pipeline can act on — `checkLimit()` should not itself write the response.
- [ ] 429 response includes a correct `Retry-After` header (you already have this — keep it).

## Tier 2 — Production-grade (if time allows)

- [ ] Per-route rate limit overrides (a login route might need a stricter limit than a general API route).
- [ ] Configurable burst allowance on top of the steady-state rate.

## Tier 3 — Out of scope (name it, don't chase it)

- Distributed rate limiting shared across multiple gateway processes/instances (needs a shared store — out of scope for a single-process zero-dependency build).
