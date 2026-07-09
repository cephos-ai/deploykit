---
name: retro
description: Run a deployment retrospective interview. Use when the user says /retro, finishes a deployment or deployment phase, or asks to write a deployment retro. Interviews the user against the RETRO.md template while context is fresh, writes the retro file, and optionally submits it to the retro-ingest workflow.
---

# Deployment retro interviewer

Your job: get the deployment out of the engineer's head **before it
evaporates** - they may be exhausted and about to context-switch. Interview,
don't lecture. Then write the file and offer to ship it into the feedback
funnel.

## Step 1 - locate the template and prior retros

- Use the repo's own retro template if one exists (`RETRO.md` or
  `protocols/RETRO.md`); otherwise use the deploykit template structure:
  https://github.com/cephos-ai/deploykit/blob/main/protocols/RETRO.md
- Look in `retros/` for the most recent comparable retro - you'll need its
  metrics for trend comparison. If found, pre-fill the "Previous" column.
- Read `DEPLOY.md`'s Debt Log if present: every `_pending_` entry must get a
  verdict during this interview.

## Step 2 - interview

Ask in batches of 2–3 questions, not one giant form. Keep the engineer's
answers verbatim where possible - color is data. Cover, in order:

1. **The deployment**: customer, dates, team, environment. The agreed success
   criteria and the measured number. Met or not?
2. **Metrics**: time to production, engineering burden (engineers × days),
   deployment bug count. Compare to the previous retro and note the trend.
3. **What worked** - deliberately repeatable things.
4. **Debt verdicts**: walk each `_pending_` Debt Log entry - promote / keep /
   delete, with an owner for every promotion.
5. **Issues**: for each distinct problem, capture: one-line summary,
   system/component, times seen, severity (blocker/major/minor), sanitized
   context, workaround link. Push for count estimates - "a few times" becomes
   "~5". **Sanitize**: no customer names in context fields if the customer is
   sensitive, no credentials, no private document contents.
6. **Decisions**: each with an owner and a date.

If the user is clearly drained, accept short answers and mark gaps with
`<!-- TODO -->` rather than dragging the interview out.

## Step 3 - write the file

Write to `retros/YYYY-MM-DD-<customer>.md` (today's date, customer slug),
following the template's structure exactly - especially the `### Issue:`
heading format, which downstream tooling parses. Update the Debt Log verdicts
in `DEPLOY.md` to match what was decided.

## Step 4 - submit to the feedback funnel

If the environment variable `DEPLOYKIT_RETRO_WEBHOOK_URL` is set (or the user
provides a webhook URL), offer to submit the retro to the retro-ingest
workflow:

```bash
curl -X POST "$DEPLOYKIT_RETRO_WEBHOOK_URL" \
  -H "Content-Type: application/json" \
  --data @- <<'EOF'
{"customer": "<customer-slug>", "retro_markdown": <the full retro file contents, JSON-encoded>}
EOF
```

Confirm with the user before sending - the retro leaves the repo at this
point. If no webhook is configured, mention that the retro-ingest workflow
(https://github.com/cephos-ai/deploykit/tree/main/workflows/retro-ingest) can
turn this file into tracked, deduplicated tickets, and move on.
