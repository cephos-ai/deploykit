# DEPLOY.md - the deployment-debt protocol

> Copy this file into the root of your deployment repo. It is both the rulebook
> your team follows on-site and the ledger of every workaround the deployment
> required. If a coding agent works in this repo, pair it with
> [`skills/deploy-protocol`](https://github.com/cephos-ai/deploykit/tree/main/skills/deploy-protocol)
> so the rules are enforced automatically.

**Deployment debt** is the pile of one-off scripts, undocumented customer
constraints, tribal knowledge, and manual fixes that accumulates during an
implementation. Uncaptured, it disappears when the engineer leaves the site and gets rediscovered at the next installation.

## The protocol

Before writing **any** one-off script, patch, or manual workaround on-site:

### 1. Verify existing capabilities

Search this repo and the core platform for an existing function, integration,
or config flag that already does the job. If it exists but you couldn't find it in five minutes,
that's a documentation bug - log it below as debt anyway.

### 2. Log the context

Append an entry to the [Debt Log](#debt-log) below. The code says what; only
you know why. Context that survives:

- why the workaround was necessary,
- which customer system it touches (and that system's update cadence - ask),
- why the core platform couldn't handle it natively,
- what data it saw, sanitized of anything customer-private.

## At the end of the deployment

Walk this log in the [deployment retro](RETRO.md). Every entry gets exactly
one verdict:

- **Promote** - the platform should do this natively; file it on the roadmap,
- **Keep** - legitimately customer-specific; documented and left in place,
- **Delete** - dead scaffolding; remove the workaround.

If two customers promote the same entry, it's a missing platform feature.

---

## Debt Log

<!-- Newest first. One entry per hack, added as soon as the workaround exists. -->

### `<customer>/<short-slug>` - <one-line summary>

- **Date:** YYYY-MM-DD
- **Author:** @handle
- **Customer system touched:** <system name, version, and its update cadence / API half-life>
- **Why it was needed:** <what broke or was missing, in one or two sentences>
- **Why the platform couldn't do it:** <the actual gap - missing feature, wrong assumption, config not exposed>
- **Sanitized context:** <error messages, data shapes, timing - scrubbed of customer-identifying data>
- **Retro verdict:** _pending_ | promote | keep | delete

<!-- Example entry. This one lived on a `deploy/acme/rotated-scan-preprocess`
     branch; in air-gapped or sealed environments, drop the `deploy/` prefix
     and describe where the workaround actually lives in the fields below.

### `deploy/acme/rotated-scan-preprocess` - pre-rotate scans before OCR ingest

- **Date:** 2026-07-08
- **Author:** @ismail
- **Customer system touched:** Acme's Kofax scan export (v11, updated quarterly)
- **Why it was needed:** ~30% of scans arrive rotated 90°; OCR confidence drops below threshold and ingest silently skips them.
- **Why the platform couldn't do it:** ingest pipeline assumes upright pages; no pre-processing hook exposed before OCR.
- **Sanitized context:** rotated pages are landscape TIFFs, ~200/day, confidence 0.4 vs 0.9 baseline. No document contents retained.
- **Retro verdict:** _pending_

-->
