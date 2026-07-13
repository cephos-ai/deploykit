---
name: deploy-protocol
description: Enforce the DEPLOY.md deployment-debt protocol when writing one-off scripts, patches, or workarounds in a customer deployment repo. Use whenever work in this repo produces code that exists because of a specific customer environment rather than the core product - hacks, glue scripts, manual fixes, config overrides.
---

# Deployment-debt protocol

You are working in a customer deployment repo. Any one-off code written here
is **deployment debt** and must be logged as soon as you write it.

## What to follow

Read `DEPLOY.md` at the root of this repo and follow "The protocol" and
"Rules of thumb" sections verbatim - required log fields, verdicts,
sanitization, small-hack rule, unlogged-debt rule. That file is the source of
truth for the workflow; this skill is just the trigger.

## Agent-specific behavior

- When you search for existing capabilities (step 1 of the protocol), tell
  the user what you searched and what you found. The human has no other way
  to verify.
- If the user asks you to skip the protocol, comply, but list the unlogged
  debt at the end of your response so it isn't silently lost.
