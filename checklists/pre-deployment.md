# Pre-deployment checklist

> Fork this. Delete what doesn't apply, add what your last deployment taught
> you.
>
> Items are tagged with the risk they defend against:
> **[process]** **[translation]** **[resilience]** **[update]** **[drift]**

## Before you arrive

### Success criteria
- [ ] Success criteria for this deployment are **quantified and written down** (a specific number and threshold)
- [ ] The customer has **agreed to that number**
- [ ] You know who on the customer side declares success, and they'll be present

### Know the environment **[translation]**
- [ ] Customer SOPs and technical specs are in the hands of the **engineers going on-site**
- [ ] Network reality confirmed: cloud / VPN / on-prem / air-gapped, and what that means for your tooling
- [ ] Access requested and confirmed: accounts, badges, VPN credentials, machine provisioning - with a named contact for when they don't work
- [ ] For every internal system you'll integrate with, you've asked about its **update cadence** **[drift]**

### Prepare for failure **[resilience]**
- [ ] Contingency written for your platform's top 3 failure modes in *this* environment
- [ ] Offline/degraded plan exists if the environment is restricted (model weights, dependencies, and docs travel with you)
- [ ] Rollback: you know how to put the customer back to the state you found them in

### Update mechanics **[update]**
- [ ] You know how you'll ship changes *during* the deployment (pipeline, cadence, who approves)
- [ ] You know how you'll ship changes *after* you leave
- [ ] Scope-change process is agreed with the customer: who approves in-flight requirement changes, and where they get written down

### Instrumentation **[process]**
- [ ] `DEPLOY.md` protocol file is in the deployment repo ([template](../protocols/DEPLOY.md))
- [ ] Feedback capture is live: Slack triage bot or equivalent ([workflow](../workflows/slack-linear-triage))
- [ ] Metrics baseline recorded: when does the clock start for time-to-production?

## Day one

- [ ] Clock started - note the timestamp access was granted
- [ ] Access actually works
- [ ] Smoke test: the smallest end-to-end path through your system runs in their environment
- [ ] Introduce the feedback channel to the customer team: where to report, what happens when they do
- [ ] Re-validate success criteria with the people in the room (what was agreed may have shifted)

## Before you leave

- [ ] Success criteria measured and the number shared with the customer
- [ ] Every workaround has a Debt Log entry in `DEPLOY.md` **[process]**
- [ ] On-call/support handoff: the customer knows who to contact, your team knows what was deployed
- [ ] Update path tested once end-to-end from outside the building **[update]**
- [ ] Metrics recorded: time to production, engineering burden, deployment bug count
- [ ] Retro scheduled within 48 hours, platform team invited ([template](../protocols/RETRO.md))
