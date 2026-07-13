---
name: deploy-protocol
description: Enforce the DEPLOY.md deployment-debt protocol when writing one-off scripts, patches, or workarounds in a customer deployment repo. Use whenever work in this repo produces code that exists because of a specific customer environment rather than the core product - hacks, glue scripts, manual fixes, config overrides.
---

# Deployment-debt protocol

You are working in a customer deployment repo. Any one-off code written here is **deployment debt** and must be logged in DEPLOY.md as soon as it exists. Follow this protocol whenever you are about to write (or have just written) a workaround, glue script, patch, or manual fix that exists because of this customer's environment.

## The protocol

1. **Verify existing capabilities first.** Search this repo and the core
   platform for an existing function, integration, or config flag that already
   does the job. Tell the user what you searched and what you found. If a
   capability exists but was hard to find, that's a documentation gap - log it
   as debt anyway.

2. **Log it in DEPLOY.md.** Append an entry to the Debt Log in the repo's
   `DEPLOY.md` following the entry template there. The entry must answer:
   - why the workaround was necessary,
   - which customer system it touches (ask the user for its update cadence if unknown),
   - why the core platform couldn't handle it natively,
   - sanitized context - **scrub all customer-identifying data** (names, IDs,
     document contents, credentials) before it goes in the log.

   Set the retro verdict to `_pending_`.

If the repo has no `DEPLOY.md`, offer to create one from the deploykit
template (https://github.com/cephos-ai/deploykit/blob/main/protocols/DEPLOY.md)
before proceeding.

## Rules of thumb

- A five-minute fix still gets a log entry.
- Never write customer-private data into the log or any commit messages.
  When in doubt, describe the shape of the data, not the data.
- If the user asks you to skip the protocol, do it, but note the unlogged
  debt at the end of your response so it isn't silently lost.
