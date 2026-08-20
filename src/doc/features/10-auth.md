# security/auth.js

**nginx equivalent:** `auth_basic` / `auth_request`

**Purpose:** Gate access to routes that require it. Depends only on `config.js`. **Must run after route matching in the pipeline** — auth should never be the reason an undefined path returns 401 instead of 404.

## Tier 1 — Core (required)

- [ ] API key check via a header (`X-API-Key` or similar) against configured valid keys.
- [ ] Clean `{ authenticated, reason }` return shape the pipeline uses to decide 401 vs proceed.
- [ ] Per-route auth requirement: not every route should be forced to require auth if the config says otherwise.

## Tier 2 — Production-grade (if time allows)

- [ ] HMAC-signed tokens with expiry (you already have this — verify expiry is actually checked, not just signature).
- [ ] Distinguish auth failure reasons in logs (missing key vs invalid key vs expired token) without leaking that detail to the client response.

## Tier 3 — Out of scope (name it, don't chase it)

- Full JWT verification (claims, issuer, audience).
- OAuth2 proxy / delegated auth flow.
