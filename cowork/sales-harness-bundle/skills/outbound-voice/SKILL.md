---
name: outbound-voice
description: How to write outbound sales messages in the [YOUR_FIRM] firm voice. Use when the Composer is drafting any outbound email, reply, or templated cadence step. Anchored on a Voice DNA artifact extracted from real best-performing messages. Includes opener/follow-up/objection frames in message-templates.md and a quarterly re-extraction procedure in voice-dna-extract.md.
cowork:
  category: writing
  icon: Edit
---

# Outbound Voice

Every Composer draft must read like it was written by a [YOUR_FIRM] human, not by a model. This skill is the source of truth for that voice.

## Files in this skill

- `voice-dna.md` — the extracted voice profile. Tone, rhythm, vocabulary, opens, closes, banned phrases. **This is THE source of truth.** Ships with `PLACEHOLDER` content — you MUST run the extraction prompt below before the Composer can draft.
- `voice-dna-extract.md` — the prompt that turns a corpus of past messages into `voice-dna.md`. Re-run quarterly or whenever reply rate drifts.
- `message-templates.md` — opener, follow-up, and backup frames. Use as starting structure only; rewrite every line in voice.

The extraction corpus (real past [YOUR_FIRM] messages used as input) lives in OneDrive, not in the skill. It contains PII and never enters the skill folder.

## Quality bar

A draft passes the voice test if a reviewer cannot tell whether it was written by a [YOUR_FIRM] human last quarter or by the model today. If they can tell, add 10 more real messages to the corpus and re-run `voice-dna-extract.md`.

## What the Composer does with this skill

1. Open `voice-dna.md`. Confirm it is not a placeholder. If it contains `PLACEHOLDER`, stop and tell the operator to extract first.
2. Pick the appropriate frame from `message-templates.md`:
   - first touch (no prior thread)
   - reply-warm (positive reply)
   - reply-objection (skeptical reply)
3. Rewrite the frame line-by-line so the result honors `voice-dna.md`.
4. Scan for banned phrases (see `compliance/banned-phrases.md` AND the banned-phrases section of `voice-dna.md`).
5. Save the draft (either to the prospect MD file's `## Draft (pending approval)` section, or directly to Outlook Drafts via `CreateDraftMessage` — see `outlook-ops/SKILL.md`).

## Structural complement: no-ai-slop

The bans below and `voice-dna.md` cover slop *words*. They do not cover slop
*structure*. The shared `no-ai-slop` skill (`cowork/no-ai-slop/`) is the
structural complement: it catches binary contrasts, colon reveals,
faux-insight setups, fake-profound endings, and robotic rhythm. The Composer
runs it as a final pass after the voice draft (see `composer/SKILL.md`). Use
the two together: `voice-dna.md` defines *how we sound*, `no-ai-slop` removes
*how machines sound*. They are not interchangeable.

## Anti-patterns

- "I hope this finds you well"
- "I came across your profile and was impressed by..."
- "I wanted to reach out because..."
- "Quick question..."
- Any subject line with the word "Quick" or "Touching base"
- Any opener that does not name a specific signal from the dossier
