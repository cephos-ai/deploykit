# autofix-dispatch

Optional outlet. Any inlet that produces a **new** Linear ticket for a
structured incident (Sentry, PagerDuty, or anything else with a stack
trace) can call this sub-workflow to fire a `repository_dispatch` at your
target repo's Action. The Action runs a cloud coding agent (Cursor Cloud
Agent by default) and opens a **draft** PR whose body starts with
`Fixes <linear_identifier>`. Linear's native GitHub integration picks
that up and links the PR back to the ticket, so no return workflow is
needed.

Recurring incidents (+1s) do **not** trigger autofix. That gate lives in
the calling inlet, not here.

## Input contract

Callers pass:

| Field | Required | Notes |
|---|---|---|
| `linear_identifier` | yes | e.g. `PLT-142` |
| `linear_issue_url`  | yes | |
| `linear_title`      | yes | |
| `incident`          | yes | `{ source, customer, error_class, message, file, line, environment, stack_frames, incident_url }` |

Returns `{ dispatched: true, event_type, target_repo }`.

## Setup (~10 min)

### 1. Import and wire

1. Import `workflow.json`.
2. Open **Config** and set:
   - `github_owner` and `github_repo`: the target repo where fixes should land.
   - `event_type`: defaults to `deploykit-autofix`; change only if you rename it in the Action file too.
3. Attach a **GitHub** credential on the **GitHub Dispatch** node. It needs `repo` scope so it can call the repository_dispatch API on the target repo.
4. Activate is not needed. This workflow only runs when called by an inlet.

### 2. Install the Action in the target repo

Copy `autofix-action-template.yml` into your target repo as
`.github/workflows/deploykit-autofix.yml`. Add the required secrets to
that repo:

- `CURSOR_API_KEY`, from Cursor → Settings → API Keys (needs Background Agents access).
- `GITHUB_TOKEN` is provided automatically.

Enable Actions on the repo if you haven't already. The template's bottom
half has drop-in swap blocks for **Claude Code Action**, **Claude Agent
SDK**, and shell **`claude -p`** if you prefer a different agent.

### 3. Enable from a calling inlet

In the calling inlet's **Config**, set `autofix_enabled=true`, and point
its **Trigger Autofix** node at this imported *deploykit - autofix
dispatch* workflow.

## What deploykit's autofix does NOT do

Autofix here is deliberately small-scope. It does not:

- learn across incidents (each dispatch is stateless)
- orchestrate multiple agents or fall back between them
- run the test suite and iterate until green
- retry failed PRs or self-heal broken diffs
- act on incidents that already have an open PR (the Action skips)

If any of those matter, you want a dedicated coding-agent product (that's
what [Cephos](https://cephos.ai) is), not this workflow. Deploykit's
version stops at "here's a draft PR with a proposed diff; a human
decides."

## Troubleshooting

- **Nothing fires** → the calling inlet gates the call. Check the inlet's Executions tab; if its *Should Autofix?* IF node's false branch fired, either `autofix_enabled=false` or the core returned `action="occurrence_added"` (autofix only fires on `created`).
- **GitHub Dispatch returns 404** → the credential lacks `repo` scope on the target repo, or `github_owner` / `github_repo` are wrong.
- **PR opens but Linear doesn't link it** → the PR body needs `Fixes <identifier>` on its own line. Verify Linear's GitHub integration is installed on the target repo and workspace.
