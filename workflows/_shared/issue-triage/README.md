# issue-triage (core sub-workflow)

The shared funnel every inlet calls. Give it a raw field report; it returns a
Linear ticket that represents the **root cause** - either an existing one that
just got a +1, or a new one.

```
input → extract & sanitize (LLM) → search Linear → same root cause? (LLM)
      → yes: +1 occurrence comment          → no: create ticket, then +1 comment
```

Deduplication happens at the **root-cause level**, not by string matching: an
LLM compares the normalized report against the top search candidates and asks
"would fixing that ticket fix this report?" So "OCR fails on rotated scans"
reported three ways from three customers lands on one ticket with three
occurrence comments.

## Input contract

Callers (the [Slack inlet](../../slack-linear-triage), the
[retro inlet](../../retro-ingest), or anything you build) pass:

| Field | Required | Notes |
|---|---|---|
| `source` | yes | `slack`, `retro`, or your own |
| `customer` | yes | deployment/customer slug |
| `raw_text` | yes | the report, verbatim - sanitization happens here, not in the caller |
| `link` | no | URL back to the original report |
| `reported_at` | no | ISO timestamp |

It returns `{ action: "occurrence_added" | "created", identifier, issue_url, title }`.

## The occurrence convention

Every occurrence - including the first, on a freshly created ticket - is a
Linear comment that starts with the literal marker `[occurrence]`:

```
[occurrence] deployment: acme · source: slack · 2026-07-08

> sanitized context of what happened

Original report: https://…
```

Occurrences for a ticket = count of its `[occurrence]` comments. That's what
the [weekly digest](../../occurrence-digest) aggregates. No labels, no
priority mutation - humans read the count and decide.

## Setup (~10 min, do this before the inlets)

1. **Import** `workflow.json` into n8n (Workflows → Import from File).
2. **Config node**: set `linear_team_id` - in Linear, Team settings → copy
   the team ID (a UUID). New tickets are created in this team.
3. **Credentials**:
   - `Search Linear`, `Create Linear Issue`, `Add Occurrence Comment` → your
     **Linear** credential (personal API key from Linear → Settings → API).
   - `OpenAI Chat Model` → your **OpenAI** credential. Different provider?
     Delete this one node and drop in any other chat-model node (Anthropic,
     Google, Ollama…) - it's the only thing you swap.
4. **Activate** is not needed - this workflow only runs when called by an
   inlet. Import the inlets next and point their *Run Issue Triage* node here.

## Design decisions (change them if you disagree)

- **Comment-only "+1", no priority changes.** The workflow never mutates
  priority; auto-bumping is one IF-node away if you want it, but we default to
  trust.
- **Sanitize at the source.** The extraction prompt strips customer-identifying
  data before anything is written to Linear. Tighten the prompt for stricter
  regimes (it's in the `Extract & Sanitize` node).
- **On Linear Business+?** Linear's native *Customer Requests* is a better +1
  primitive (sortable request counts). Swap the `Add Occurrence Comment`
  mutation for `customerNeedCreate` and you can drop the digest workflow.

## Troubleshooting

- **"Workflow could not be started"** from an inlet → the inlet's
  *Run Issue Triage* node isn't pointed at this imported workflow.
- **Everything creates new tickets** → check `searchIssues` returns results
  for your workspace (run this workflow alone with pinned test data); very
  new workspaces with empty backlogs will behave this way - correctly.
- **Duplicate-ish tickets** → the judge is conservative by design; loosen the
  root-cause definition in the `Judge Root Cause` prompt.
