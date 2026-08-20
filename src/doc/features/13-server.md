# core/server.js

**nginx equivalent:** master process + listener sockets

**Purpose:** Boot the HTTP and HTTPS listeners on top of `pipeline.js`, sharing one context between them. Depends on `pipeline.js` and `tls.js`.

## Tier 1 — Core (required)

- [ ] `http.createServer` and `https.createServer` **share the exact same request-context instance** (same load balancer, health checker, WAL, metrics, rate limiter) — never construct the context twice, or you get duplicate health-check polling and two WAL writers racing on one file.
- [ ] Graceful shutdown: stop health checker, flush WAL, drain in-flight connections with a timeout, then close (you already have this pattern — keep it as the reference for future modules).

## Tier 2 — Production-grade (if time allows)

- [ ] Upstream connection keep-alive: use an `http.Agent({ keepAlive: true })` for outbound requests to backends instead of opening a fresh TCP connection per proxied request — small change, real performance claim.
- [ ] Multi-process via `node:cluster` — a master process forking N workers, each running this same server, sharing listen sockets. This is the single highest-signal addition if you have the hours: it's nginx's actual master/worker architecture, and Node's `cluster` module gets you there almost for free.

## Tier 3 — Out of scope (name it, don't chase it)

- HTTP/2 support.
- Zero-downtime binary upgrade (nginx's exec-replace-on-SIGUSR2 trick) — genuinely deep systems work, don't attempt it.
