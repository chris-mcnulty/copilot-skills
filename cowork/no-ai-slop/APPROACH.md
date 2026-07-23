# Evaluation and integration approach: no-ai-slop

How the `no-ai-slop` skill is assessed and how it plugs into the Sales Harness
bundle and the Marketing Skills library to raise output quality.

Source: `no-ai-slop` by Peter Yang, MIT-licensed. See `SOURCE.md`.

## 1. Evaluation

### What it is

A single editing skill with two modes. **Edit** makes the minimum effective
change to a draft and returns the result plus a "What changed" note. **Detect**
names the AI patterns it finds, quotes the offending line, and gives a short
fix, without rewriting or guessing whether a machine wrote the text. It ships
with `eval.md`, a pass/fail checklist the skill runs against its own edits
before returning them.

### Strengths

- **It catches structure, not just words.** Binary contrasts ("it's not X,
  it's Y"), colon reveals, faux-insight setups ("what most people miss"),
  trailing "-ing" pseudo-analysis, fake-profound kickers, summary-recap
  endings, and robotic rhythm are the tells that make competent AI prose read
  as AI. A word blocklist cannot catch any of them.
- **Voice-preserving by design.** The first instruction is to notice and keep
  the writer's cadence, bluntness, humor, and uncertainty, and to make the
  minimum edit. That fits our bundles, where voice is the whole point of the
  outbound harness and the marketing copy.
- **Detect mode gives evidence, not a verdict.** It names a pattern and quotes
  the line, so a human can check the call. That is more useful than an AI
  detector's score and safe to run on a colleague's draft.
- **Self-checking.** The `eval.md` companion means the skill grades its own
  output against fixed criteria instead of stopping at "looks good."

### Limits and cautions

- **It is an editing pass, not a generator.** It improves a draft that already
  exists. It does not know our ICP, proof points, or offer. It runs *after* a
  drafting skill, never instead of one.
- **Judgment-based, not a hard gate.** The rules say "cut when empty, keep when
  it carries the writer's rhythm." That nuance is a strength for prose and a
  mismatch for the harness's current absolute hard-fail blocklist. See §4.
- **Some rules are context-dependent.** The em-dash rule and a few word bans
  disagree with rules already in our skills. Those are listed in §4 for a
  decision rather than resolved here.
- **No M365 grounding.** Unlike our bundle skills, it reads no SharePoint or
  Outlook context. It operates only on the text handed to it. That is fine for
  its job and means it adds no new tool dependencies.

### Verdict

Adopt it as a shared, final-stage quality gate across both bundles. It is
additive: it covers the structural layer neither bundle addresses today, and it
preserves voice rather than flattening it. Keep it vendored unchanged so it
stays easy to re-sync from upstream.

## 2. The gap it fills

Both bundles already fight generic output, but only at the word and format
level, and unevenly.

| Layer | Sales Harness today | Marketing today | no-ai-slop |
|---|---|---|---|
| Banned words / clichés | `compliance/banned-phrases.md` (hard fail), voice-DNA bans | inline lists in `copywriter`, `repurposing-engine` | Yes, plus a larger list with keep/cut nuance |
| Voice match | `outbound-voice/voice-dna.md` | `brand/` voice guidelines | Preserves voice, does not define it |
| **Structural AI tells** | **not covered** | **not covered** | **Yes (its core value)** |
| Detect / audit mode | none | none | Yes |
| Self-check against fixed criteria | partial (composer redraft, copywriter self-check) | partial | Yes (`eval.md`) |

The structural row is the point. Our skills can strip "leverage" and "game
changer" and still ship a draft that opens with "Here's the thing," reveals its
point after a dramatic colon, and closes on a mic-drop aphorism. That draft
reads as AI, and nothing in either bundle catches it today.

## 3. Integration approach

### Principle

`no-ai-slop` runs as the **last step before a draft is surfaced to a human**,
after the drafting skill has done its own voice and compliance work. It never
replaces a skill's existing rules. Where a skill already has a rule, that rule
stays authoritative; `no-ai-slop` adds the structural checks on top. It runs in
**edit** mode inside the pipeline and is available in **detect** mode for
on-demand audits of human or AI drafts.

Because the skill is judgment-based and our harness compliance layer is an
absolute hard-fail gate, the two are kept in the right order: **compliance
hard-fail first (redraft on any hit), then no-ai-slop structural polish.** The
polish pass must never reintroduce a banned phrase, so compliance re-runs after
it in the harness.

### Sales Harness

The prose-producing surfaces and how each changes:

- **`composer`** (first-touch and reply emails). Add a final structural pass
  after the voice draft and the banned-phrase scan: run the `no-ai-slop`
  structural checks (binary contrasts, colon reveals, faux-insight setups,
  superficial "-ing" analysis, fake-profound kickers, summary-recap endings,
  robotic rhythm, importance puffery, weasel attribution, synonym cycling).
  Re-run the compliance banned-phrase scan after it. This is the highest-value
  wiring: a cold email that reads as AI gets deleted, and these patterns are
  exactly what a busy prospect pattern-matches on.
- **`outbound-voice`** (voice authority). Note that `no-ai-slop` is the
  structural complement to the word-level voice-DNA bans, so the two are used
  together and not confused for each other.
- **`cadence-rules`** (templated follow-ups). These templates are authored once
  and reused. Run `no-ai-slop` in detect mode when *writing or revising* a
  template, not on every send. The current `outbound-email-v1` steps are
  already tight; this keeps future templates clean.

Left untouched by decision: `compliance/banned-phrases.md` stays an absolute
hard-fail gate; `no-ai-slop`'s voice-preserving word handling neither overrides
nor adds to it (see §4, conflict B).

### Marketing Skills

- **`copywriter`** (LinkedIn, blog, newsletter, X, email, video). Add the
  structural checks to the existing self-check block, scoped to patterns not
  already covered by the skill's rules. This is the second-highest-value
  wiring after the composer.
- **`repurposing-engine`** (8-10 assets per long-form piece). Add the
  structural checks to the output guardrails. Repurposing multiplies one
  source into many assets, so one slop pattern in the source becomes ten in
  the output. Catching it here has leverage.
- **`content-strategist`** (briefs and differentiation angles). Light touch:
  apply the checks to the prose fields (topic titles, differentiation angles)
  so briefs don't seed slop downstream. The JSON structure is unaffected.
- **`branded-doc-generator` / `branded-deck-generator`.** These format
  confirmed drafts and are told never to add filler. Apply `no-ai-slop` only to
  any *connective* text they generate (section headings, callout summaries),
  never to rewrite the confirmed body. Document only; no rule change needed.
- **`positioning-researcher`, `seo-aeo-optimizer`, `performance-analyst`,
  `distribution-planner`.** Analysis and scheduling outputs. Detect mode is
  available on request. No pipeline wiring. Note that SEO metadata and
  `no-ai-slop` can pull in opposite directions (keyword repetition vs. synonym
  cycling and puffery), so SEO rules win on metadata fields.

### What the wiring looks like in a skill

Each wired skill gets a short block that (a) points to the shared skill, (b)
lists the structural patterns to apply, and (c) states explicitly that the
skill's own existing rules remain authoritative and that the conflicts in §4
are not yet resolved. No existing rule text is edited.

## 4. Conflicts requiring your decision

Per your instruction to decide per conflict, these are flagged and **left
unresolved**. The existing rules stay in force until you rule. Nothing below
has been changed in the skills.

### Conflict A: em-dash policy (RESOLVED)

**Decision:** em dashes are banned outright, in every format and length. Edit
every one out; a colon, comma, or plain hyphen is fine. This is a house rule
and it overrides `no-ai-slop`'s "1-2 in longer drafts" allowance.

**Applied:** the vendored `no-ai-slop` `SKILL.md` and `eval.md` were edited to
the outright ban, marked inline as a house override and recorded in `SOURCE.md`.
The `composer`, `copywriter`, and `repurposing-engine` notes state the resolved
rule. No further action.

Background: the two rules only ever disagreed on long-form (blog, newsletter);
for LinkedIn, X, and email they already agreed on none.

### Conflict B: absolute blocklist vs. judgment-based editing (RESOLVED)

**Decision:** voice wins. `no-ai-slop`'s word list is treated as *default cuts*,
not an absolute blocklist. Words like "utilize" and "facilitate" that are part
of a writer's natural cadence are kept, because stripping them makes the writer
sound less like themselves, not more human. `no-ai-slop`'s larger word list is
**not** folded into the Sales Harness `compliance/banned-phrases.md` hard-fail
gate.

**Applied:** the "Words to cut" list in `no-ai-slop`'s `SKILL.md` was relabeled
"default cuts" with a house-override note; the `eval.md` word check was
softened to match; both recorded in `SOURCE.md`. The `composer` note states
that `no-ai-slop`'s word handling is voice-preserving and does not add to the
compliance gate.

**Untouched:** `compliance/banned-phrases.md` stays an absolute hard-fail gate.
It never listed the writer's natural words (no "utilize", no "facilitate"), so
the two rules do not actually collide. The genuinely bad clichés it bans
(game-changer, transformative, synergy, and the outbound clichés) remain
hard-fails.

### Conflict C: the word "harness" (RESOLVED)

**Decision:** "harness" is a plain English word ("to put to use") and is not
slop. It was removed from `no-ai-slop`'s word list. No other action.

## 5. How to verify it is working

- **A/B a real draft.** Take one recent composer email and one copywriter
  LinkedIn post. Run each through detect mode. If it names zero patterns, the
  upstream skills were already clean. If it names several, that is the value,
  visible.
- **Reply-rate watch (sales).** The harness already re-extracts voice DNA when
  the 30-day reply rate drops 5 points. Treat a sustained reply-rate lift after
  turning on the structural pass as the signal it is helping. Do not over-read
  short-term noise.
- **Human read (marketing).** The copywriter self-check already asks "does this
  sound like a person or a brand account." The structural pass makes that check
  pass more often. Spot-check weekly.

## 6. Rollout

1. Land the shared skill (this folder) with attribution. Done.
2. Wire `composer`, `copywriter`, and `repurposing-engine` first (highest
   leverage). Additive references only; existing rules untouched.
3. Add lighter references to `outbound-voice`, `cadence-rules`, and
   `content-strategist`.
4. Conflicts A (em dashes), B (word list), and C ("harness") are all resolved.
   See §4 for each decision.
5. Leave the analysis and formatting skills on detect-mode-on-request.
