# Pre-deployment checklist

> Fork this. Delete what doesn't apply, add what your last deployment taught
> you.
>
> Items are tagged with the risk they defend against:
> **[process]** **[translation]** **[resilience]** **[update]** **[drift]**

## Before you arrive

### Success criteria
- [ ] Success criteria for this deployment are **quantified and written down** (a specific number and threshold)
- [ ] The customer has **agreed to that number** - in writing, before anyone travels
- [ ] You know who on the customer side declares success, and they'll be present

### Know the environment **[translation]**
- [ ] Customer SOPs and technical specs are in the hands of the **engineers going on-site** - not summarized through a PM
- [ ] Network reality confirmed: cloud / VPN / on-prem / air-gapped, and what that means for your tooling
- [ ] Access requested and confirmed: accounts, badges, VPN credentials, machine provisioning - with a named contact for when they don't work
- [ ] For every internal system you'll integrate with, you've asked about its **update cadence and API half-life** **[drift]**

### Prepare for failure **[resilience]**
- [ ] Contingency written for your platform's top 3 failure modes in *this* environment
- [ ] Offline/degraded plan exists if the environment is restricted (model weights, dependencies, and docs travel with you)
- [ ] Rollback: you know how to put the customer back to their pre-you state

### Update mechanics **[update]**
- [ ] You know how you'll ship changes *during* the deployment (pipeline, cadence, who approves)
- [ ] You know how you'll ship changes *after* you leave
- [ ] Requirements will shift on-site - the update path is ready **before** momentum depends on it

### Instrumentation **[process]**
- [ ] `DEPLOY.md` protocol file is in the deployment repo ([template](../protocols/DEPLOY.md))
- [ ] Feedback capture is live: Slack triage bot or equivalent ([workflow](../workflows/slack-linear-triage)) - set up **before** the first bug, not after
- [ ] Metrics baseline recorded: when does the clock start for time-to-production?

## Day one

- [ ] Clock started - note the timestamp access was granted
- [ ] Access actually works
- [ ] Smoke test: the thinnest end-to-end slice of your system runs in their environment
- [ ] Introduce the feedback channel to the customer team: where to report, what happens when they do
- [ ] Re-validate success criteria with the people in the room (what was agreed may have shifted)

## Before you leave

- [ ] Success criteria measured and the number shared with the customer - **do not leave the building without it**
- [ ] Every workaround is on a `deploy/<customer>/<slug>` branch with a Debt Log entry - nothing lives only on a customer machine **[process]**
- [ ] On-call/support handoff: the customer knows who to contact, your team knows what was deployed
- [ ] Update path tested once end-to-end from outside the building **[update]**
- [ ] Metrics recorded: time to production, engineering burden, deployment bug count
- [ ] Retro scheduled within 48 hours, platform team invited ([template](../protocols/RETRO.md))
