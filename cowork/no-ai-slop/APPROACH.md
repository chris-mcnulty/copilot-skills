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

Left untouched pending a decision: `compliance/banned-phrases.md` (see §4,
conflict B).

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

### Conflict A: em-dash policy

- **`copywriter` and `repurposing-engine`:** "No em dashes." Absolute.
- **`no-ai-slop`:** none in short copy, 1-2 allowed in longer drafts when they
  clearly beat a comma or period.
- **Where they actually disagree:** only long-form (blog, newsletter). For
  LinkedIn, X, and email they agree (use none).
- **Options:** (1) keep the absolute ban everywhere, ignore no-ai-slop's
  allowance; (2) adopt no-ai-slop's "1-2 in long-form" and relax the ban for
  blog/newsletter only; (3) something in between (e.g. ban in social, allow in
  blog).
- **Recommendation:** option 1 for now. The absolute ban is simpler to enforce
  and the cost is low.

### Conflict B: absolute blocklist vs. judgment-based editing

- **`compliance/banned-phrases.md`:** any listed phrase is a hard fail, redraft.
  No exceptions. This is a compliance gate.
- **`no-ai-slop`:** many of the same words, but "cut when empty, keep when it
  carries emphasis, uncertainty, or the writer's rhythm."
- **The tension:** if we let no-ai-slop's nuance override the harness gate, a
  banned word could survive on a judgment call. That weakens a compliance
  control.
- **Options:** (1) keep the harness blocklist absolute and authoritative;
  no-ai-slop's nuance applies only to words *not* on the compliance list;
  (2) fold no-ai-slop's larger word list into the compliance blocklist as new
  hard-fail terms (stricter); (3) replace the hard-fail gate with judgment
  (looser, not recommended for outbound).
- **Recommendation:** option 1, and optionally option 2 for the clearly-slop
  additions (delve, foster, facilitate, streamline, robust, paradigm shift,
  tapestry, realm, beacon). Keep the gate absolute.

### Conflict C: the word "harness"

`no-ai-slop` bans "harness" as a banned-outright word. Our product is literally
the "Sales Harness." This only matters in customer-facing *output copy*, never
in internal skill names or docs. No action needed unless "harness" appears in a
draft sent to a prospect. Flagged for completeness.

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
4. Resolve conflicts A and B once you rule on §4, then update the affected
   rules if needed.
5. Leave the analysis and formatting skills on detect-mode-on-request.
