# workflows

n8n workflows. Need a running n8n first:

```bash
docker run -it --rm -p 5678:5678 -v ~/.n8n:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

Open http://localhost:5678, then import each `workflow.json` (Workflows → Import from File). Per-workflow setup lives in each folder's README.

**Layout:**

- [`_shared/issue-triage`](_shared/issue-triage) - the core all inlets call. **Import first.**
- `inlets/` - one per source. Each takes an event, sanitizes it, and calls the core.
  - [`inlets/slack`](inlets/slack)
  - [`inlets/retro`](inlets/retro)
  - [`inlets/observability`](inlets/observability)
- `outlets/` - what fires after triage.
  - [`outlets/occurrence-digest`](outlets/occurrence-digest) - weekly Slack digest of top recurring issues.
  - [`outlets/autofix-dispatch`](outlets/autofix-dispatch) - optional, called by inlets with structured incident shape; fires a draft-PR autofix via a cloud coding agent.

**Import order (if using everything):**
1. `_shared/issue-triage`
2. `outlets/autofix-dispatch` (only if wiring autofix; inlets need to point at it)
3. inlets (`slack`, `retro`, `observability`)
4. `outlets/occurrence-digest`
