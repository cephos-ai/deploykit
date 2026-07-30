# slack

@mention a bot on any Slack message worth tracking. The workflow pulls the
whole thread, sanitizes it, and runs it through the
[triage core](../../_shared/issue-triage): **+1** on the ticket with the same
root cause, or a **new ticket** if there isn't one. The bot replies in-thread
with what it did:

> :heavy_plus_sign: +1 on **PLT-142** (same root cause) - OCR fails on rotated scans
> https://linear.app/…

Use it when someone hits a bug on-site: mention the bot, keep working, the report is in Linear.

## Setup (~10 min, after the [triage core](../../_shared/issue-triage))

### 1. Create the Slack app

At [api.slack.com/apps](https://api.slack.com/apps) → *Create New App* → from scratch:

- **OAuth scopes (Bot Token):** `app_mentions:read`, `channels:history`,
  `channels:read`, `chat:write`
- Install to workspace, copy the **Bot User OAuth Token** into an n8n Slack
  credential.

### 2. Import and wire

1. Import `workflow.json`.
2. Point **Run Issue Triage** at your imported *deploykit - issue triage (core)* workflow.
3. Attach the Slack credential on the trigger and the three Slack nodes.
4. Open **Slack Trigger**, copy its production webhook URL, and paste it into
   your Slack app under *Event Subscriptions* → enable → Request URL. Then
   subscribe to the bot event `app_mention`.
5. Activate the workflow.

### 3. Use it

- Name deployment channels `deploy-<customer>` (e.g. `deploy-acme`) - the
  customer slug is derived from the channel name. Other channel names work
  too; the whole name becomes the slug.
- `/invite @yourbot` to the channel.
- Mention the bot on the message (or anywhere in the thread) you want
  tracked: `@deploybot this is the third time ingest silently skipped a batch`.
  The **entire thread** is used as context, so mentioning after discussion helps.

## Customization

- **Trigger on 🎫 reactions instead of mentions:** swap the trigger event for
  `reaction_added` and filter on your emoji - everything downstream is
  unchanged.
- **Don't reply in-thread:** delete the last node.
- **Multiple Linear teams:** duplicate the workflow per team, or route on
  channel name before *Run Issue Triage* and pass a team override.
