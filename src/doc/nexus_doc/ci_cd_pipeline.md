Test cases = spec for judges' confidence. CI = proof they pass on both OS automatically. Split: test case list here (chat), workflow file created (deliverable).

## Test cases per module (what to assert)

**config.js** — valid config loads correctly · missing required field (`backends`/`listen`) throws · malformed JSON throws · defaults fill in when optional fields absent

**tls.js** — cert auto-gen when `certs/` empty · clear error when `openssl` missing from PATH (mock `execSync` failure) · https server actually binds with generated cert

**logger.js** — log level filtering works (debug suppressed in prod mode) · log format matches spec · no throw on malformed input

**metrics.js** — `recordRequest()` updates counters correctly · error rate = errors/total, not miscounted · per-route breakdown isolates correctly · `/nexus/metrics` returns valid JSON

**router.js** — exact match · prefix match · longest-prefix wins when overlapping · host-based match · unmatched path returns `null` (not throw)

**loadbalancer.js** — round-robin cycles all backends evenly · skips backend marked unhealthy · weighted distribution respects weight ratio · least-conn picks lowest active count

**healthcheck.js** — marks unhealthy only after N consecutive fails (not 1) · marks healthy again after N consecutive successes · status readable externally

**wal.js** — append writes valid entry · batched flush fires on interval, not per-write · rotation triggers at size threshold · old files pruned to keep last N

**ratelimiter.js** — allows under limit · blocks over limit with 429 · `Retry-After` value correct · bucket resets after window elapses

**auth.js** — valid API key passes · invalid/missing key fails · HMAC token valid/invalid/expired

**pipeline.js (integration, mock the leaves)** — static routes (`/`, `/nexus/metrics`) skip auth entirely · unmatched path → 404, never 401 · rate limit checked before route match · auth checked after route match, only on matched routes · unhealthy backend → 502 · `metrics.recordRequest` called on **every** exit branch (this is the regression test for the bug you hit)

**server.js (real, not mocked)** — boot on ephemeral port, real `http.request` round-trip returns 200 · backend down → 502, not hang · request/response body streams correctly for large payloads

**cli.js** — `start --config <path>` boots · invalid config path exits non-zero with message

Skip unit tests for `backend-echo.js`, `start.js`, `index.html` — fixtures/orchestration/static, not logic under test.

## GitHub Actions — file needed

`.github/workflows/ci.yml`, matrix across Mac + Windows (+ Linux free bonus).Workflow references `scripts/ci-smoke-test.js` — needed since bash/curl smoke tests break on Windows runners. Creating it too:## Files needed, exact paths

```
.github/workflows/ci.yml     ← drop into repo root exactly at this path
scripts/ci-smoke-test.js      ← alongside your existing scripts/start.js
```

## Steps in GitHub

1. Commit both files at those paths, push to `main` or open a PR.
2. GitHub auto-detects `.github/workflows/ci.yml`, no manual setup in repo settings needed.
3. Actions tab → see matrix run: 3 OS × 2 Node versions = 6 parallel jobs + 1 config-validation job.
4. Green check on PR = judges/teammates see pass/fail before merge, no manual "did you test this" trust needed.

Two things this workflow assumes exist, build these next if not already:

- **`npm test` script** in `package.json` → `"test": "node --test"` (Node's built-in test runner, zero-dep, matches your constraint)
- **`cli.js -t` flag** — config-validate-only mode, referenced in `config-test` job. If not built yet, that job fails until you add it (Tier 2 item from the parity doc, worth bumping to Tier 1 now since CI depends on it).

Fix `taskkill`/`SIGTERM` split in smoke test handles the Windows orphan-process risk directly — same class of bug as your `EADDRINUSE` issue earlier, killed at CI level instead of surfacing during a live judge demo.