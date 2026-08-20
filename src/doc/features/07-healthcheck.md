# reliability/healthcheck.js

**nginx equivalent:** active health checks (nginx Plus) / passive checks (OSS)

**Purpose:** Continuously monitor backend liveness so `loadbalancer.js` never routes to a dead backend. Depends on `config.js` and `logger.js`.

## Tier 1 — Core (required)

- [ ] Active polling: hit each backend's health endpoint on a configurable interval.
- [ ] Unhealthy threshold: mark a backend unhealthy only after N consecutive failures (not on the first blip — avoids flapping).
- [ ] Expose current health status per backend for `loadbalancer.js` and the dashboard to query.
- [ ] Log a clear line on every state transition (`healthy → unhealthy`, `unhealthy → healthy`) — this is exactly the kind of line a judge scans for during a live demo.

## Tier 2 — Production-grade (if time allows)

- [ ] **Passive** checks: mark a backend unhealthy immediately if a real proxied request to it fails (502/connection refused), independent of the polling cycle — faster reaction than waiting for the next poll.
- [ ] Recovery threshold: require N consecutive successful polls before marking a previously-unhealthy backend healthy again (prevents flapping on recovery too).

## Tier 3 — Out of scope (name it, don't chase it)

- Circuit breaker with exponential backoff on retry interval for a persistently-down backend.
