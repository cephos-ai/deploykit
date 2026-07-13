# deploy-protocol skill

Makes coding agents follow the [DEPLOY.md protocol](../../protocols/DEPLOY.md)
while you hack on-site: verify existing capabilities before writing a
workaround, and log the context in the Debt Log in the same commit, with
customer data scrubbed.

## Install

**Claude Code** - copy the skill folder into the deployment repo (applies to
everyone working in it):

```bash
mkdir -p .claude/skills
cp -r path/to/deploykit/skills/deploy-protocol .claude/skills/
```

or into `~/.claude/skills/` to apply to everything you personally work on.

**Cursor / other agents** - use [`rules.md`](rules.md), a copy-paste variant
of the same protocol for `CLAUDE.md`, `.cursor/rules/`, or `AGENTS.md`.

## What it changes in practice

With the skill, an agent asked to "fix the OCR ingest for these rotated scans":

1. searches the platform for an existing preprocessing hook first,
2. appends a sanitized Debt Log entry to `DEPLOY.md` in the same commit as the workaround,
3. leaves the retro verdict `_pending_` for the [retro](../../protocols/RETRO.md) to resolve.

The deployment repo needs a `DEPLOY.md` at its root - grab the
[template](../../protocols/DEPLOY.md). If it's missing, the skill offers to
create it.
