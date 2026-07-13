# retro-ingest

A [daily deployment retro](../../protocols/RETRO.md) goes in; every issue in
it comes out the other side of the
[triage core](../_shared/issue-triage) - deduplicated against Linear at the
root-cause level, +1'd or filed. Day 3's retro re-reporting Day 1's OCR bug lands as
a second +1 on the same ticket, not a duplicate.

What gets extracted:

- every `### Issue:` block (the template's structured format), and
- every Debt Log entry the retro **promoted**.

## Three ways in

| Door | Effort | When to use |
|---|---|---|
| **Form** | zero - open the Retro Form node's URL and paste | daily retros written in Docs/Notion, or a paste right after a standup |
| **Webhook** | zero - `POST { customer, retro_markdown }` | the [`/retro` skill](../../skills/retro) submits here automatically |
| **GitHub push** | pick owner/repo + GitHub credential | retros committed to `retros/*.md` in your deployment repo trigger the workflow on push |

All three converge on the same normalization step; use whichever fits how
your retros actually happen, or all of them at once.

## Setup (~5 min, after the [triage core](../_shared/issue-triage))

1. Import `workflow.json`.
2. Point **Run Issue Triage** at your imported *deploykit - issue triage (core)*.
3. Attach your OpenAI credential on the model node (or swap it - see the core's README).
4. Activate. Grab the form URL from **Retro Form** and the webhook URL from
   **Retro Webhook** (for `DEPLOYKIT_RETRO_WEBHOOK_URL` if you use the skill).
5. *(Optional, GitHub door)*: select owner/repository on **GitHub Push** and
   **Fetch Retro File** and attach a GitHub credential. The trigger registers
   its own webhook on the repo.

## Notes

- The webhook door is unauthenticated by default - if your n8n is
  internet-facing, add header auth on the **Retro Webhook** node and pass the
  header from the skill's curl.
- `times_seen` from the retro is carried into the ticket context but produces
  **one** occurrence comment per retro (a retro is one sighting event); the
  weekly [digest](../occurrence-digest) counts sighting events, not raw
  frequency.
