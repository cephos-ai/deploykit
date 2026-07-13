# retro skill

`/retro` runs a **daily deployment retro**. Either interviews one engineer, or ingests a paste of meeting
notes / a chat scroll / individual notes from multiple engineers - whatever
your team actually does. Writes `retros/YYYY-MM-DD-<customer>.md` from the
[RETRO.md template](../../protocols/RETRO.md), updates Debt Log verdicts in
`DEPLOY.md`, and - if you've wired up the
[retro-ingest workflow](../../workflows/retro-ingest) - submits the retro so
every issue lands in Linear, deduplicated against what's already there
(including yesterday's retro from the same customer).

## Install

```bash
mkdir -p .claude/skills
cp -r path/to/deploykit/skills/retro .claude/skills/
```

Then run `/retro` in the deployment repo at the end of each day, at a
milestone, or as the final wrap-up.

## Wiring it to the funnel (optional)

Set the webhook URL from your imported
[retro-ingest workflow](../../workflows/retro-ingest):

```bash
export DEPLOYKIT_RETRO_WEBHOOK_URL="https://your-n8n/webhook/deploykit-retro"
```

With it set, the skill offers to POST the finished retro; the workflow splits
it into discrete issues and runs each through the
[triage core](../../workflows/_shared/issue-triage) - +1 on existing tickets,
new tickets for new root causes. Without it, you still get the retro file;
you can paste it into the workflow's form later.
