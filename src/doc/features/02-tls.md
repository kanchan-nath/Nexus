# security/tls.js

**nginx equivalent:** `ssl_certificate` / `ssl_certificate_key`, TLS termination

**Purpose:** Terminate HTTPS at the gateway so backends only ever speak plain HTTP. Self-contained — no internal imports needed, safe to write early.

## Tier 1 — Core (required)

- [ ] Auto-generate a self-signed cert + key via `openssl` on first run if none exists at the configured path (checked-in certs are a red flag in a repo anyway).
- [ ] Fail loudly with the exact manual `openssl` command to run if `openssl` isn't on `PATH` — never crash silently mid-boot.
- [ ] Wire the cert/key into `https.createServer`, reusing the **same request pipeline/context** as the HTTP listener (don't spin up a second set of health-check timers or a second WAL writer).

## Tier 2 — Production-grade (if time allows)

- [ ] **SNI-based multi-cert support**: serve different certs per hostname if `routes` config maps multiple domains to the same gateway.
- [ ] Warn in logs when a cert is within N days of expiry.

## Tier 3 — Out of scope (name it, don't chase it)

- OCSP stapling.
- Mutual TLS (client certificate verification).
- HTTP/2 via ALPN negotiation.
