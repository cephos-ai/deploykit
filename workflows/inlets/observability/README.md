# observability

Sentry and PagerDuty incidents land here. Each one is shape-mapped
(allowlist, so PII stays out by construction), deduplicated by
fingerprint, and passed to the [triage core](../../_shared/issue-triage):
**+1** on the ticket with the same root cause, or a **new ticket** if
there isn't one.

When `autofix_enabled=true` and the incident produces a **new** ticket,
*Trigger Autofix* calls the
[autofix-dispatch outlet](../../outlets/autofix-dispatch) with the
Linear identifier plus the sanitized incident shape. Recurring incidents
(+1s) never trigger autofix.

## Setup (~10 min, after the [triage core](../../_shared/issue-triage))

### 1. Import and wire

1. Import `workflow.json`.
2. Point **Run Issue Triage** at your imported *deploykit - issue triage (core)* workflow.
3. Open **Config** and set `autofix_enabled`: leave `false` for the first end-to-end test; flip to `true` after Linear tickets look right.
4. Activate.

### 2. Point Sentry at the inlet

In your Sentry project → **Alerts** → **Create Alert Rule** → *Issues* →
action *Send a notification via an Integration → Webhooks*. Add a webhook
integration pointing at the **Sentry Webhook** node's production URL.
Optional: append `?customer=<slug>` to override the customer name
(default is the Sentry project slug).

### 3. Point PagerDuty at the inlet

In your PagerDuty service → **Integrations** → **Add** → *Generic
Webhooks (v3)*. Subscribe to the `incident.triggered` event and point it
at the **PagerDuty Webhook** node's production URL. Same
`?customer=<slug>` override applies.

### 4. Enable autofix (optional)

Only if you flipped `autofix_enabled=true`: import
[`outlets/autofix-dispatch`](../../outlets/autofix-dispatch) (with its
own Config + GitHub credential + Action template in the target repo),
then point *Trigger Autofix* at the imported *deploykit - autofix
dispatch* workflow. Its README has the details.

## Sanitization

Sentry and PagerDuty payloads carry user emails, request bodies, and
arbitrary tags. The **Shape-Map** node uses an **allowlist**, not a
scrubber: only known fields (error class, message, file, line, in-app
stack frames, environment, occurrence count, incident URL) reach the
core. Everything else is dropped by construction. If you need extra
fields, add them explicitly to the allowlist.

## Troubleshooting

- **Nothing happens on webhook fire** → check the workflow's Executions tab. If Shape-Map filters everything out, your payload doesn't match either the Sentry (`data.issue` or `data.event`) or PagerDuty (`event.data`) shape. Print `$input.all()` in a temporary Code node to inspect.
- **Autofix never dispatches** → confirm `Config.autofix_enabled` is `true`, and that the core returned `action: "created"` (autofix only fires on new tickets, never +1s). The *Should Autofix?* IF node's output tab shows which branch fired. Everything past that gate belongs to [autofix-dispatch](../../outlets/autofix-dispatch).
- **Same incident triaged twice** → the fingerprint dedup TTL is 1h. Sentry reopens on regressions with the same issue id; PagerDuty escalations reuse the dedup_key. If a legit second incident is being swallowed, drop the TTL in the **Fingerprint Dedup** node.
