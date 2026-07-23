# Source and attribution

This skill is **not** original Synozur work. It is a third-party skill vendored
into this repository unchanged.

- **Skill:** `no-ai-slop`
- **Author:** Peter Yang
- **Upstream:** https://github.com/petergyang/no-ai-slop
- **License:** MIT (see the `LICENSE` file in this folder; © 2026 Peter Yang)
- **Write-up:** https://creatoreconomy.so/p/use-my-no-ai-slop-skill-to-remove-20-ai-slop-patterns

## Licensing note

The rest of this repository is licensed under Apache-2.0 (© The Synozur
Alliance LLC). This folder is the exception: `SKILL.md` and `eval.md` are
Peter Yang's work under the MIT License, and the MIT copyright and permission
notice travel with them in the accompanying `LICENSE` file. Keep that notice
in place in any copy or redistribution.

Synozur's contribution is the *integration*: the plan for wiring this skill
into the Sales Harness and Marketing Skills bundles, documented in
`APPROACH.md`. That integration guidance is Synozur work under the repo's
Apache-2.0 license. The skill content itself was authored by Peter Yang.

## What was changed

`SKILL.md` and `eval.md` are carried over from the upstream repository with two
documented deviations, so the skill stays easy to re-sync.

**1. Em-dash rule (Synozur house override).** Upstream allows 1-2 em dashes in
longer drafts. Synozur bans em dashes outright in any length of copy (edit
every one out; use a colon, comma, or plain hyphen instead). This changes one
bullet in `SKILL.md` ("Patterns to cut" -> "Em dashes") and one line in
`eval.md` (check 8 under "Patterns to cut"). Both are marked inline.

**2. Word list yields to voice (Synozur house override).** Upstream calls the
first word list "banned outright." Synozur treats it as *default cuts* that
voice preservation overrides: a word on the list that is genuinely part of the
writer's natural cadence (for example some writers' "utilize" or "facilitate")
is kept, not stripped. The word "harness" was removed from the list entirely
(plain English for "put to use"). This changes the "Words to cut" heading and
adds an override note in `SKILL.md`, and softens check 1 under "Words to cut"
in `eval.md`. Both are marked inline. This keeps the skill consistent with its
own top principle (preserve the writer's real voice) and does not touch the
Sales Harness `compliance/banned-phrases.md` hard-fail gate, which stays
absolute.

When re-syncing from upstream, re-apply both overrides.

No other Synozur-specific rules were merged into the skill. All bundle-specific
integration lives in the consuming skills and in `APPROACH.md`, not here.
