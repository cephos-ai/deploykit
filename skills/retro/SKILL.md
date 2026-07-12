---
name: retro
description: Run a daily deployment retrospective. Use when the user says /retro, finishes a day of a deployment, hits a checkpoint or milestone, or asks to write a deployment retro. Accepts either an interview with one engineer or a paste of meeting notes / a chat scroll / individual notes from multiple engineers. Writes the retro file and optionally submits it to the retro-ingest workflow.
---

# Deployment retro interviewer

Your job: get **today's deployment work** out of the engineers' heads before
tomorrow overwrites it. Retros are a daily habit here, not an end-of-flight
ritual. Then write the file and offer to ship it into the feedback funnel.

## Step 1 - locate the template and prior retros

- Use the repo's own retro template if one exists (`RETRO.md` or
  `protocols/RETRO.md`); otherwise use the deploykit template structure:
  https://github.com/cephos-ai/deploykit/blob/main/protocols/RETRO.md
- Look in `retros/` for the most recent retro (usually yesterday's for the
  same customer) to check continuity: which issues were open, which debt was
  still `_pending_`, what decisions were due today.
- Read `DEPLOY.md`'s Debt Log entries added since the last retro - each one
  needs a verdict today.

## Step 2 - pick an input mode

Ask which shape the raw input takes, and adapt:

- **Interview** (one engineer, live): default. Ask in batches of 2-3
  questions, not one giant form. Keep answers verbatim where possible.
- **Meeting notes / chat scroll / shared doc paste**: user pastes the raw
  text. Extract structure from it; ask short follow-ups only for missing
  fields (severity, count, sanitization gaps).
- **Multiple engineers, async**: collect what each contributed, then
  synthesize into one retro file. Record every contributor by name in the
  Contributors field.

## Step 3 - cover the fields

Whichever mode, walk these in order:

1. **Deployment**: customer, today's date, checkpoint (day N of ~M, or
   milestone name, or end-of-deployment), contributors, anything new about
   the environment.
2. **What worked today** - things worth repeating tomorrow. Small wins count.
3. **Debt verdicts**: walk each new `_pending_` Debt Log entry - promote /
   keep / delete, with an owner for every promotion.
4. **Issues seen today**: for each distinct problem, capture: one-line
   summary, system/component, times seen **today**, severity
   (blocker/major/minor), sanitized context, workaround link. Push for count
   estimates - "a few times" becomes "~3". **Sanitize**: no customer names in
   context fields if the customer is sensitive, no credentials, no private
   document contents. Don't worry about deduping against prior days - the
   funnel handles that.
5. **Decisions**: each with an owner and a date.
6. **End-of-deployment retro only**: also fill Agreed success criteria + Met?
   using the SoW verbatim.

If a contributor is clearly drained, accept short answers and mark gaps with
`<!-- TODO -->` rather than dragging the retro out.

## Step 4 - write the file

Write to `retros/YYYY-MM-DD-<customer>.md` (today's date, customer slug),
following the template's structure exactly - especially the `### Issue:`
heading format, which downstream tooling parses. Update the Debt Log verdicts
in `DEPLOY.md` to match what was decided.

## Step 5 - submit to the feedback funnel

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
