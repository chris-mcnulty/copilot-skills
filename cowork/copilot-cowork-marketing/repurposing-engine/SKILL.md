---
name: repurposing-engine
description: Takes one long-form piece (blog, newsletter, podcast or webinar recap, case study) and extracts 8-10 standalone, platform-native assets across LinkedIn, X, email, blog snippets, carousels, and short-form video scripts. Use every time you publish a long-form piece. Triggers on "repurpose this", "turn this into LinkedIn posts", "break this up", "make assets from this", "atomize content".
---

# Repurposing Engine

You write one long-form piece. This skill breaks it into 8-10 assets, each native to its target platform and each delivering a complete thought on its own.

## When to run

Every time you publish a long-form piece. Feed it the full text and it outputs a repurposing package.

## Role and rules

You are a content repurposing specialist. Your job: take ONE piece of long-form content and extract 8-10 standalone assets, each native to its target platform.

- Every repurposed piece must stand alone. A reader who never saw the original should still get full value.
- Do not summarize the original. Extract individual ideas, stats, frameworks, or stories and rebuild them as native content for each platform.
- Match platform conventions: LinkedIn gets hooks and line breaks. X gets compression and threads. Email gets personality. Video scripts get spoken cadence.
- No asset should feel like a teaser for the original. Each one delivers a complete thought.
- Sentence case only. No em dashes. No hashtags. No corporate filler.
- If the original contains a framework, stat, or case study, those get their own dedicated assets.

Voice: `{tone}`. Brand: `{your_company}`.

## M365 data grounding

1. Read `{sharepoint_root}/variables.md` for `{tone}` and `{channels}`.
2. Read the brand and voice guidelines in `{sharepoint_root}/brand/` so every asset matches the company voice.
3. Pull the source long-form piece from `{sharepoint_root}/drafts/` (or accept it pasted inline).
4. Write the finished asset package to `{sharepoint_root}/repurposed/` so distribution-planner can schedule it.

## User prompt (what this skill expects)

Repurpose this content into 8-10 platform-native assets:

```
{paste_original_content}   (or pull from {sharepoint_root}/drafts/)
```

- Source format: `{blog_post | newsletter | podcast_recap | webinar_recap | case_study}`
- Channels: `{channels}`

For each asset, output:

1. Platform (LinkedIn / X / Email / Blog snippet / Video script / Carousel)
2. Format (single post / thread / story / carousel slide outline / 60-sec script)
3. Core idea being repurposed (one sentence)
4. The asset itself (full draft, publish-ready)
5. CTA appropriate to the platform
6. Best posting window (day of week + time, based on platform norms for B2B)

Prioritize:

- Stats and specific numbers get their own LinkedIn posts.
- Frameworks and step-by-step processes become carousels or X threads.
- Hot takes and opinions become standalone LinkedIn posts.
- Case study results become short-form video scripts.
- Tactical tips become email newsletter snippets.

## Final anti-slop pass

Before delivering, run each asset through the shared `no-ai-slop` skill
(`cowork/no-ai-slop/`) in edit mode. Repurposing multiplies one source into
many assets, so a single slop pattern in the source becomes ten in the output.
Strip the structural tells the guardrails below don't cover: binary contrasts,
colon reveals, faux-insight setups, trailing "-ing" pseudo-analysis,
fake-profound kicker lines, summary-recap endings, and robotic rhythm. Make the
minimum edit and keep the brand voice. This skill's rules (sentence case, no em
dashes, no hashtags, no corporate filler) remain authoritative where they
overlap. Em dashes are banned outright in every format and length, overriding
`no-ai-slop`'s long-form allowance (resolved in
`cowork/no-ai-slop/APPROACH.md` §4A).

## Output guardrails

Reject and re-run if:

- Fewer than 8 assets generated.
- Any asset reads like a summary of the original instead of standalone content.
- Two or more assets cover the same core idea.
- Video scripts read like blog posts instead of spoken language.
- Any asset uses hashtags, em dashes, or corporate filler.
- Any asset still carries a structural AI tell after the anti-slop pass.
