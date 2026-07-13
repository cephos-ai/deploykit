# RETRO.md - deployment retrospective template

> **Run this every day of a deployment**, not just at the end. One file per
> day at `retros/YYYY-MM-DD-<customer>.md` in your deployment repo. The final
> day's retro doubles as the wrap-up (fill in the success-criteria fields
> then).
>
> The input can be a stand-up, a shared doc the team added to during the day,
> a chat scroll, or one engineer's notes - whatever your team actually does.
> Contributors go in the field below; it does not have to be a meeting.
>
> Two shortcuts: run the
> [`/retro` skill](https://github.com/cephos-ai/deploykit/tree/main/skills/retro)
> to interview you (or ingest a paste of meeting notes), and/or feed the
> finished file to the
> [`retro-ingest` workflow](https://github.com/cephos-ai/deploykit/tree/main/workflows/retro-ingest)
> so every issue below lands in your tracker automatically.

## Deployment

- **Customer:** <name or codename>
- **Date:** <today>
- **Checkpoint:** <day N of ~M / milestone: <name> / end-of-deployment>
- **Contributors:** <engineers who fed this retro - on-site, remote, present in the meeting, or async in the doc>
- **Environment:** <cloud / on-prem / air-gapped; anything unusual or newly discovered today>
- **Agreed success criteria:** <fill only on the end-of-deployment retro - copy verbatim from the SoW>
- **Met?** <fill only on the end-of-deployment retro> yes / no / partially - <the number>


## What worked today

-

## Deployment debt review

Walk the [DEPLOY.md](DEPLOY.md) Debt Log entries logged **since the last
retro** (or all of them, on the end-of-deployment retro). Every entry gets a
verdict - promote (platform should do this), keep (legitimately
customer-specific), or delete (dead scaffolding). Record the promotions here;
they are roadmap input.

| Debt Log entry | Verdict | Owner | Notes |
|---|---|---|---|
| `<customer>/<slug>` | promote / keep / delete | @handle | |

## Issues

<!--
One block per distinct problem encountered today, including ones you worked
around. Keep the structure: the retro-ingest workflow splits on "### Issue:"
headings and reads the fields. Sanitize context: describe the shape of the
data, not the data - no customer names, IDs, private document contents,
credentials.

Recurrence across days is handled downstream: the same root cause on day 1
and day 3 lands as two +1s on one ticket, not two tickets. So just report
what you saw today.
-->

### Issue: <one-line summary>

- **System/component:** <which part of the platform or integration>
- **Times seen today:** <count or estimate>
- **Severity:** blocker / major / minor
- **Context (sanitized):** <what happened, error shapes, timing - enough for an engineer who wasn't there>
- **Workaround:** <link to Debt Log entry if one exists, or "none">

## Decisions

<!-- Every decision needs an owner and a date. -->

| Decision | Owner | By when |
|---|---|---|
| | | |
