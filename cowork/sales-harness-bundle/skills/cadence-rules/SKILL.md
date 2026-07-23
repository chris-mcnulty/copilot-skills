---
name: cadence-rules
description: When to follow up, what to send, and when to stop on [YOUR_FIRM] outbound. Defines pre-approved templated cadence steps the Cadence skill can queue under guardrails. Use when running the Cadence loop, deciding whether to queue the next step for a lead, or interpreting a reply. Anchored on timing.md (business-hour windows), templates/ (pre-approved cadence copy), and stop-conditions.md.
cowork:
  category: automation
  icon: Clock
---

# Cadence Rules

The Cadence skill uses this to decide:

1. When to fire the next step (timing in `timing.md`)
2. What content to send (templates in `templates/`)
3. When to stop bothering a prospect (`stop-conditions.md`)

## Files in this skill

- `timing.md` — business-hour windows, timezone rules, business-day math
- `templates/` — pre-approved cadence step templates, each in its own file with a stable ID. Changes to a template are versioned via filename (`outbound-email-v2.md`).
- `stop-conditions.md` — the rules that move a lead to `dormant`

## The contract with the Composer

The Composer authors first-touch and reply drafts that require human approval. The Cadence skill only queues content from `templates/`. That's the "pre-approved at design time" promise: each template was reviewed when added; the queueing is acceptable because the wording already passed review.

If a template needs a tweak for a specific prospect, Cadence **does not tweak**. It saves the rendered version to Outlook Drafts (via `CreateDraftMessage` or `CreateReplyDraft`) with `state: draft_pending_approval` and a `hold_reason: needs personalization` so the Composer (or a human) can handle it.

In v1, the human still clicks Send for every templated step. Auto-send is a future flag.

## Template format

Each file in `templates/` describes **one complete cadence** (a series of steps under a single campaign), not a single step. The frontmatter identifies the cadence and enumerates its steps:

```yaml
---
id: outbound-email-v1            # stable cadence ID — referenced by
                                  # prospect frontmatter as
                                  # cadence_template_id
campaign: outbound-email-v1      # campaign tag (usually equals id)
description: Default outbound cadence for cold prospects.
steps:
  - id: outbound-email-v1-step-2 # stable per-step ID
    days_after_previous: 3
    channel: email
  - id: outbound-email-v1-step-3
    days_after_previous: 4
    channel: email
  - id: outbound-email-v1-step-4
    days_after_previous: 7
    channel: email
variables:                       # variables any step in this cadence
  - first_name                   # may reference
  - hook_summary
---
```

Below the frontmatter, the body has one `## Step N` section per step in `steps`, containing the templated copy for that step. Variables are referenced as `{first_name}` etc. Cadence fills variables from the prospect's MD file frontmatter and dossier sections.

When a prospect's frontmatter says `cadence_template_id: outbound-email-v1`, Cadence looks up `templates/outbound-email-v1.md`, finds the next step (per `steps[]` and the lead's `touch_count`), and renders the corresponding `## Step N` body section.

## Adding a new template

1. Create a new file under `templates/` (e.g. `outbound-email-v2.md`).
2. Run the copy through the shared `no-ai-slop` skill (`cowork/no-ai-slop/`) in
   detect mode and fix anything it names. Templates are written once and reused
   across every lead on that cadence, so a slop pattern here ships hundreds of
   times. This is a design-time check on the template author, not a per-send
   step (Cadence never rewrites a template at send time).
3. The voice owner reviews; another rep reviews.
4. After review, the template ID is available for new campaigns. Existing in-flight leads keep using the template version they started on (immutable cadence — changing mid-stream changes the experiment).
