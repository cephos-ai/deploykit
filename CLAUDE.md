# CLAUDE.md

deploykit is an open-source kit for deployment engineering: protocols, workflows, checklists, and agent skills for teams putting AI systems into production inside customer environments. Audience is forward-deployed engineers (FDEs) at AI solution providers. It is the reference implementation of the essay "Deployment Is All You Need" (https://cephos.substack.com/p/deployment-is-all-you-need). Maintained by Cephos, MIT licensed.

## Repo layout

- `protocols/`: DEPLOY.md and RETRO.md templates. Markdown, meant to be copied into a customer deployment repo and forked.
- `checklists/`: pre-deployment checklist mapped to the essay's five risk categories.
- `workflows/`: n8n workflow exports. `inlets/` (slack, retro, observability), `_shared/issue-triage` (the core), and `outlets/` (occurrence-digest, autofix-dispatch).
- `skills/`: Claude Code and Cursor agent skills. `deploy-protocol` enforces DEPLOY.md; `retro` runs the retrospective interview.

## Writing conventions

**Audience already has the pain.** Do not write persuasion, mission statements, or essay-style thesis paragraphs. Every artifact reads as utility, not advocacy. If a section is convincing the reader that deployment matters, cut it. The essay is the "why"; this repo is the "what and how."

**Show artifacts, not concepts.** Prefer screenshots, example outputs, and real code samples over concept diagrams. A picture of the Linear ticket the workflow produces beats a mermaid flowchart of the pipeline.

**Terse and direct.** No corporate voice. No "we're excited to." Short paragraphs. If a line does not earn its place, delete it.

**No aphorisms, slogans, or X-not-Y tails.** Cut phrases like "that's the point," "humans read it and decide," "not just X, Y," "or it's a wish," "small wins count" the moment they appear. Definitions, instructions, and checklists don't need editorial closers.

**No metaphor vignettes or buzzword adjectives.** Debt doesn't "evaporate," workflows don't "come out the other side," day one doesn't "die," code isn't "agent-native," APIs don't have "half-lives." State the fact or consequence plainly.

**Never use em dashes.** Use hyphens, commas, colons, or parentheses instead. Applies to every file in the repo, including this one.

**Forkable over configurable.** Artifacts should be plain markdown or single-file workflow exports. Readers change the words, not the config.

**Sanitize at the source.** Any workflow or skill that touches customer context must scrub identifiers before the context leaves its origin channel. Canonical phrasing for prose: "describe the shape of the data, not the data."

**Skills reference, don't restate.** Protocol and template content lives in `protocols/*.md` (the files users fork). `SKILL.md` files load them and add only agent-specific behavior (triggers, interview mechanics, scaffolding, submission). Never re-enumerate fields or restate rules. `skills/deploy-protocol/rules.md` is the one deliberate mirror, for non-Claude agents that can't chain file reads.

## When editing the README

The README's job, in order: does this fit my stack, what will I see when it works, what do I copy first. Lead with the artifact table and a hero visual, not with a thesis section. Do not restate the essay's arguments.

## When adding a new artifact

1. It has to be useful standalone (design principle #1).
2. Add a row to the artifact table in the README with the file path, a one-line "what it does," and the format.
3. If it is a workflow, add it under `workflows/inlets/` or `workflows/outlets/` (whichever fits), and add a setup section to `workflows/_shared/issue-triage/README.md` or the new folder's own README.
