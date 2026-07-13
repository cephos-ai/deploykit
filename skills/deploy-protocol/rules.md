# Deploy-protocol rules (copy-paste variant)

Not using Claude Code skills? Paste the block below into whatever your agent
reads: `CLAUDE.md`, `.cursor/rules/deploy-protocol.mdc`, `AGENTS.md`, or a
system prompt. Same protocol, no skill machinery.

---

```markdown
## Deployment-debt protocol

This is a customer deployment repo. For ANY one-off script, patch, glue code,
or workaround that exists because of this customer's environment (rather than
the core product):

1. BEFORE writing it: search this repo and the core platform for an existing
   capability that already does the job. Report what you searched.

2. Append an entry to the Debt Log in DEPLOY.md covering: why it was needed,
   which customer system it touches (and that system's update cadence), why
   the core platform couldn't do it natively, and sanitized context. Scrub
   ALL customer-identifying data - describe the shape of the data, not the
   data.

Small hacks get logged too. If asked to skip the protocol, comply but state
what debt went unlogged.
```
