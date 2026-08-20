# routing/loadbalancer.js

**nginx equivalent:** `upstream` block

**Purpose:** Given a matched route, pick which backend instance handles the request. Depends on `config.js` and needs route names from `router.js`.

## Tier 1 — Core (required)

- [ ] Round-robin selection across a route's backend pool.
- [ ] Weighted round-robin (respect per-backend `weight` in config).
- [ ] **Health-aware**: never select a backend the health checker has marked unhealthy — this is the integration point with `healthcheck.js`, get the interface right early.
- [ ] Return `null` cleanly when zero healthy backends exist for a route — pipeline turns that into a 502, not this module's job.

## Tier 2 — Production-grade (if time allows)

- [ ] Least-connections strategy (track in-flight request count per backend).
- [ ] IP-hash / sticky sessions (same client IP consistently routes to the same backend).
- [ ] Live connection count exposed for the dashboard's per-backend table.

## Tier 3 — Out of scope (name it, don't chase it)

- Dynamic upstream membership changes (add/remove backend) without a full config reload.
- Slow-start (gradually ramp traffic to a backend that just recovered from unhealthy).
