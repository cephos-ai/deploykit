# RETRO.md - deployment retrospective template

> Copy this template to `retros/YYYY-MM-DD-<customer>.md` in your deployment
> repo and fill it in **within 48 hours** of finishing (or checkpointing) a
> deployment - while context is still cheap. Both the deployment team and the
> platform team should be in the room.
>
> Two ways to skip the blank page: run the
> [`/retro` skill](https://github.com/cephos-ai/deploykit/tree/main/skills/retro)
> and let it interview you, and/or feed the finished file to the
> [`retro-ingest` workflow](https://github.com/cephos-ai/deploykit/tree/main/workflows/retro-ingest)
> so every issue below lands in your tracker automatically.

## Deployment

- **Customer:** <name or codename>
- **Dates:** <start> → <end or checkpoint>
- **Team:** <engineers on the ground + remote support>
- **Environment:** <cloud / on-prem / air-gapped; anything unusual>
- **Agreed success criteria:** <the quantified outcome the customer signed off on - copy it verbatim>
- **Met?** yes / no / partially - <the number>

## Metrics

Compare against your last *comparable* deployment. If these aren't trending
down, you're not scaling - that's the point of measuring.

| Metric | This deployment | Previous | Trend |
|---|---|---|---|
| Time to production (first access → fully working, integrated system) | | | ↓ / → / ↑ |
| Engineering burden (engineers × days) | | | ↓ / → / ↑ |
| Deployment bug rate (bugs/on-call requests caused by the deployment itself) | | | ↓ / → / ↑ |

## What worked

<!-- Things to deliberately repeat next time. Checklist items that earned their place. -->

-

## Deployment debt review

Walk the [DEPLOY.md](DEPLOY.md) Debt Log. Every entry gets a verdict -
promote (platform should do this), keep (legitimately customer-specific), or
delete (dead scaffolding). Record the promotions here; they are roadmap input.

| Branch | Verdict | Owner | Notes |
|---|---|---|---|
| `deploy/<customer>/<slug>` | promote / keep / delete | @handle | |

## Issues

<!--
One block per distinct problem encountered, including ones you worked around.
Keep the structure: the retro-ingest workflow splits on "### Issue:" headings
and reads the fields. Sanitize context - no customer-private data.
-->

### Issue: <one-line summary>

- **System/component:** <which part of the platform or integration>
- **Times seen this deployment:** <count or estimate>
- **Severity:** blocker / major / minor
- **Context (sanitized):** <what happened, error shapes, timing - enough for an engineer who wasn't there>
- **Workaround:** <link to Debt Log entry if one exists, or "none">

## Decisions

<!-- Every decision needs an owner and a date, or it's a wish. -->

| Decision | Owner | By when |
|---|---|---|
| | | |
