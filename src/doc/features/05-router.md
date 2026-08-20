# routing/router.js

**nginx equivalent:** `location` blocks / `server_name`

**Purpose:** Map an incoming path (and optionally host) to a named route. Depends only on `config.js`.

## Tier 1 — Core (required)

- [ ] Exact-path matching.
- [ ] Prefix-path matching (`/api` matches `/api/users`, `/api/orders`, etc.).
- [ ] Longest-prefix-wins resolution when multiple routes could match the same path (nginx's core `location` matching rule — get this right, it's the one nginx behavior everyone half-remembers wrong).
- [ ] Host-based routing when `Host` header is used to disambiguate (optional, only if your config supports multiple virtual hosts).
- [ ] Return `null`/`undefined` cleanly for unmatched paths — the pipeline is responsible for turning that into a 404, not this module.

## Tier 2 — Production-grade (if time allows)

- [ ] Basic regex `location` matching (a JS `RegExp`-based subset, not full PCRE).
- [ ] Route-level metadata attached to the match result (e.g. per-route auth requirement, per-route rate limit override) so `pipeline.js` doesn't need a second lookup.

## Tier 3 — Out of scope (name it, don't chase it)

- Full PCRE-compatible regex semantics.
- `rewrite` directive equivalent (path rewriting before forwarding).
