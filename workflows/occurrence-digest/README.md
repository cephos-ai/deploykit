# occurrence-digest

Every Monday at 09:00, this workflow counts the `[occurrence]` comments the
[triage core](../_shared/issue-triage) filed over the past week, ranks issues
by recurrence, and posts the top 10 to a Slack channel:

> :repeat: **Top recurring deployment issues - week of 2026-07-06**
>
> 1. [PLT-142] OCR fails on rotated scans - **4×** this week (acme, globex)
> 2. [PLT-158] Ingest silently skips oversized batches - **3×** this week (acme)
> …

This closes the loop the kit exists for: a bug seen once is an anomaly; the
same root cause +1'd across deployments is a roadmap priority. The digest
makes the counts impossible to ignore without mutating anyone's priorities or
labels - humans read it and decide.

Quiet week, no `[occurrence]` comments → no post.

## Setup (~3 min)

1. Import `workflow.json`.
2. Attach your **Linear** credential on `Fetch Occurrence Comments`.
3. On `Post Digest`, pick the destination channel (your platform team's
   channel is the right audience) and attach your **Slack** credential.
4. Activate.

## Customization

- **Cadence:** edit the `Every Monday` schedule node. Bi-weekly matches
  bi-weekly platform planning.
- **Window & depth:** the GraphQL query fetches up to 250 occurrence comments
  from the last week (`-P1W`). Widen to `-P2W`/`-P1M` for slower deployment
  rhythms. If you exceed 250 occurrences a week, add pagination - and also,
  congratulations on the deployment volume.
- **Per-deployment breakdown:** the deployment slugs are already parsed from
  the comments; extend `Format Digest` to group by deployment instead of
  ranking globally.
- **On Linear Business+:** native Customer Requests give you sortable request
  counts inside Linear itself, making this digest optional - see the
  [core README](../_shared/issue-triage/README.md#design-decisions-change-them-if-you-disagree).
