# deploy-protocol skill

Makes coding agents follow the [DEPLOY.md protocol](../../protocols/DEPLOY.md)
while you hack on-site: verify existing capabilities before writing a
workaround, then log the context in the Debt Log with customer data scrubbed.

## Install

**Claude Code** - from the repo root, one install step (skill + the `DEPLOY.md`
it enforces):

```bash
mkdir -p .claude/skills && \
  cp -r path/to/deploykit/skills/deploy-protocol .claude/skills/ && \
  cp path/to/deploykit/protocols/DEPLOY.md .
```

Drop the skill into `~/.claude/skills/` instead if you want it applied to
every repo you personally work in; `DEPLOY.md` still lives at each repo's root.

**Cursor / other agents** - use [`rules.md`](rules.md), a copy-paste variant
of the same protocol for `CLAUDE.md`, `.cursor/rules/`, or `AGENTS.md`.

## What it changes in practice

With the skill, an agent asked to "fix the OCR ingest for these rotated scans":

1. searches the platform for an existing preprocessing hook first,
2. appends a sanitized Debt Log entry to `DEPLOY.md`,
3. leaves the retro verdict `_pending_` for the [retro](../../protocols/RETRO.md) to resolve.
