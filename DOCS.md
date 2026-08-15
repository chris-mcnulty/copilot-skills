# Enterprise AI Skills Documentation

This document covers all fifteen AI skill entries in this repository — nine personal skills and bundles built for **Copilot Cowork** and six organizational skills built for **Copilot in SharePoint**. Each section explains what the skill does, when to use it, how it works, and how to put it into practice.

---

## Table of Contents

**Copilot Cowork Skills**
1. [Decision Log Builder](#1-decision-log-builder)
2. [Pre-Commitment Capacity Check](#2-pre-commitment-capacity-check)
3. [Stakeholder Intelligence Brief](#3-stakeholder-intelligence-brief)
4. [Morning Briefing](#4-morning-briefing)
5. [Weekday Morning Briefing](#5-weekday-morning-briefing)
6. [Synozur Board](#6-synozur-board)
7. [Marketing Skills Bundle](#7-marketing-skills-bundle)
8. [Sales Harness Bundle](#8-sales-harness-bundle)
9. [Spot Financial Outliers](#9-spot-financial-outliers)

**Copilot in SharePoint Skills**
10. [Content Expiration Sentinel](#10-content-expiration-sentinel)
11. [Onboarding Path Synthesizer](#11-onboarding-path-synthesizer)
12. [Process Compliance First-Pass](#12-process-compliance-first-pass)
13. [Excel to Branded HTML Dashboard](#13-excel-to-branded-html-dashboard)
14. [Library Destination Advisor](#14-library-destination-advisor)
15. [SharePoint List Dashboard](#15-sharepoint-list-dashboard)

**Summary**
- [Copilot Cowork vs SharePoint Skills](#copilot-cowork-vs-sharepoint-skills)
- [How Skills Scale in an Organization](#how-skills-scale-in-an-organization)

---

# Copilot Cowork Skills

---

## 1. Decision Log Builder

### What It Is

Decision-making is continuous and scattered — across emails, meetings, and chat threads. The Decision Log Builder consolidates those dispersed decisions into a single, structured record with dates, owners, rationale, and source links. It solves the common problem of teams losing track of what was decided, by whom, and why — especially across long-running projects.

### When to Use It

- After a busy quarter, when you need a clear record of every significant call your team made
- Before a project retrospective, to reconstruct the sequence of decisions and who owned them
- When a stakeholder asks "why did we go with this approach?" and you need to find the original context quickly
- When preparing to onboard someone new to a workstream and want to give them decision history without sitting through hours of recordings
- When you notice conflicting directions across workstreams and need to surface whether a formal decision was ever made

### How It Works

The skill begins by confirming the review window — defaulting to the last 30 days if none is specified. It then searches in parallel across email, Teams messages, and calendar meetings for language that signals a finalized decision: phrases like "we decided," "agreed to," "final call," "going with," or "signed off." 

From those results, it filters out opinions, open discussions, and proposals still awaiting approval — retaining only items that establish clear direction. Each qualifying decision is captured with its date, a one-sentence summary, identifiable participants, stated rationale (never inferred), accountable owner, and a linked reference back to the original source.

Where the same decision appears across multiple sources — for example, a meeting and a follow-up confirmation email — the skill merges them into a single entry. It flags two categories of risk: decisions with no clear owner and decisions that appear to conflict across sources. The final output is a structured table followed by a short narrative on key themes, ownership gaps, and conflicting directions.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  decision-log-builder/
    SKILL.md
```

The front matter must include `name`, `description`, and `cowork` metadata with a `category` and `icon` field. No additional configuration files are required.

### How to Use It

- *"Log all decisions from the past month across my email and Teams."*
- *"What did we decide on the Henderson project? Build me a decision log."*
- *"Why was this approach approved? Show me the decision history."*
- *"What have we agreed to in the last six weeks? Capture it in a table."*
- *"Build a decision log for the Q2 planning workstream."*

### Example Output

| Date | Decision | Owner | Source |
|------|----------|-------|--------|
| May 3 | Proceed with vendor A for the data integration layer | J. Torres | [msg_12] |
| May 9 | Defer the mobile release to Q3 | Unassigned ⚠️ No owner | [evt_4], [msg_17] |
| May 14 | Approve revised scope excluding legacy migration | R. Patel | [chat_6] |

**Key Themes & Risks:** Three of seven decisions lack a named owner, creating execution risk in the integration track. Two decisions — on release scope — show conflicting language across sources and should be reviewed before the next milestone.

### Key Design Principles

- **Source-grounded output only.** Every decision entry must trace to a specific email, meeting, or chat. Nothing is fabricated or inferred.
- **Rationale is captured, not constructed.** If the reason for a decision wasn't explicitly stated in the source material, the field is left blank.
- **Flags, not fixes.** The skill surfaces ownership gaps and conflicts — it does not attempt to resolve them or assign owners on the user's behalf.

---

## 2. Pre-Commitment Capacity Check

### What It Is

Professionals routinely overcommit because they agree to new work based on intuition rather than actual availability. The Pre-Commitment Capacity Check reviews a person's real workload — calendar, sent email, and recent Teams conversations — before they say yes to a new engagement, SOW, or project. It produces a concrete recommendation with named tradeoffs, not a generic "you're busy."

### When to Use It

- Before signing a statement of work or accepting a new client engagement
- When a colleague asks if you can lead a project and you want an honest answer before responding
- When evaluating whether the current quarter can absorb an additional commitment
- Before agreeing to a delivery deadline that feels optimistic
- When a proposal or estimate has been shared and you want to stress-test whether your team can actually deliver it

### How It Works

If a document — such as a SOW, proposal, or estimate — is referenced, the skill reads it first, extracting total hours, phases, required roles, deliverables, and scope boundaries. It then compares the user's informal description of the engagement against the document's actual scope and flags any mismatch before touching the capacity math.

Next, the skill runs three parallel workload sweeps: a 30-day calendar scan to identify blocked weeks, travel, OOF periods, focus blocks, and recurring commitments; a review of the last two weeks of sent email for explicit promises ("I'll have this to you by Friday"); and a scan of recent Teams chats for open asks and in-progress threads likely to convert to real work.

From this data, the skill computes available hours per week, compares them against the engagement's requirements, and matches project phases to realistic windows. It concludes with one of three clear recommendations — take it on solo, take it on with a partner or role split, or decline and defer — accompanied by a confidence score. Anything below 75% confidence includes an explicit statement of what information is missing.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  pre-commitment-capacity-check/
    SKILL.md
```

The front matter uses a multi-line `description` block listing all trigger phrases and exclusions. The `cowork` block sets `category: analysis` and specifies an icon.

### How to Use It

- *"Can I take on the Meridian SOW? Here's the proposal."*
- *"Should I commit to leading this engagement? Check my capacity."*
- *"Is this a good time to add another workstream?"*
- *"Bandwidth check — do I have room to sign this before end of month?"*
- *"Before I agree to this, review my next 30 days."*

### Example Output

```
Recommendation (confidence ~82): Take it on with a partner — capacity exists but 
June is constrained.

Scope reality check: You described this as a "strategy redesign." The SOW defines 
it as narrative refinement and explicitly excludes rebranding. Confirm scope before 
proceeding.

Engagement: 120h / $48,000 / ~6 weeks. Phases: Discovery (20h), Workshops (40h), 
Synthesis (40h), Final delivery (20h).

Current obligation load:
- Committed: Week of June 2 fully blocked (offsite); "I'll have the report by June 7" 
  sent to M. Allen on May 28.
- Likely incoming: Open ask from D. Kim re: budget narrative (Teams, May 30 — 
  unanswered).

High-capacity weeks: June 16–20, June 23–27.

Suggested split: Delegate Discovery phase to a junior analyst; you own Workshops 
and Synthesis in the June 16 window.
```

### Key Design Principles

- **Scope before capacity.** A framing mismatch between what the user thinks they're agreeing to and what the document actually says is surfaced first — capacity math on the wrong scope is meaningless.
- **Committed work is not just what's on the calendar.** Sent-email promises and open Teams asks are treated as real obligations, even when they don't appear as meetings.
- **Specificity is non-negotiable.** The recommendation names specific weeks, dates, and roles. Generic statements like "you seem busy but it could work" are explicitly prohibited.

---

## 3. Stakeholder Intelligence Brief

### What It Is

Walking into a meeting without context is a missed opportunity. The Stakeholder Intelligence Brief scans the last 90 days of a user's email and Teams history with a specific person and produces a structured, evidence-based snapshot in two minutes. It surfaces open commitments, unresolved threads, and observable communication patterns — so the user arrives informed, not scrambling.

### When to Use It

- Before a one-on-one with an executive or client you haven't spoken to in several weeks
- When preparing for a difficult conversation and needing to know what has been promised or left open
- When picking up a relationship that a colleague has been managing and you need a quick handoff brief
- Before a quarterly review with a key stakeholder to ensure no open items are missed
- When a stakeholder contacts you unexpectedly and you want to get up to speed before responding

### How It Works

The skill begins by resolving the person's identity against the directory, surfacing candidates if the name is ambiguous rather than guessing. It then gathers relevant signals from the past 90 days across email (messages received from and sent to the person) and Teams, including direct chat history and, when available, channel mentions.

From those results, it extracts three categories of signal: commitments the user made (things they said they would do, with dates and message citations); open or unresolved items (questions from the stakeholder with no visible reply, or promised deliverables with no follow-through); and communication patterns (cadence, response gaps, and any urgent or escalation language, quoted verbatim without interpretation).

If the history is thin — fewer than roughly five messages — the skill says so plainly and summarizes only what exists. It does not pad the output with speculation. The final briefing is delivered as inline markdown with four clearly labeled sections, each grounded in cited sources.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  stakeholder-intelligence-brief/
    SKILL.md
```

The front matter uses a multi-line `description` block with trigger phrases. The `cowork` block sets `category: productivity` and specifies the `PersonInfo` icon.

### How to Use It

- *"Prep me for my meeting with Sarah Chen this afternoon."*
- *"Brief me on Marcus before our call — what's open between us?"*
- *"What have I committed to Director Wang in the last three months?"*
- *"Get me up to speed on my history with the Apex account lead."*
- *"Stakeholder brief on Jordan — we haven't spoken in a while and I need context."*

### Example Output

**Relationship Snapshot**
Sarah Chen — VP of Operations, Alliance Partners. Regular interaction over the past 90 days, averaging roughly two exchanges per week, with a notable gap in April.

**Commitments Made**
- "I'll have the updated timeline to you by Friday" — May 12 [msg_4]
- "We'll share the cost model before the board review" — April 28 [msg_11]

**Open or Unresolved Items**
- Sarah asked for confirmation on the revised scope on May 8 — no reply found [msg_7]
- Follow-up promised on Q2 resourcing (April 15) — no closing message found [msg_2]

**Communication Pattern Signals**
- Cadence: consistent weekly exchanges, except a three-week gap in April
- Escalation language present: "urgent" (May 8, [msg_7]); "please advise" (May 14, [chat_3])
- Two messages from Sarah currently unanswered

### Key Design Principles

- **Observation, not interpretation.** The skill reports what is visible in messages — it does not infer sentiment, relationship health, or intent beyond the written record.
- **Verbatim escalation signals.** Urgency language is quoted exactly as written. The skill does not characterize tone or make judgments about the relationship.
- **Honesty about data gaps.** If the history is sparse, the briefing says so — it does not fill space with generic observations.

---

## 4. Morning Briefing

### What It Is

The Morning Briefing skill composes and delivers a personalized daily email covering the five things a professional needs to start the day: relevant industry news, communications requiring a reply, priority client updates, top committed tasks, and today's schedule. It replaces a manual morning review with a single AI-generated scan-in-60-seconds email sent directly to the user's inbox.

### When to Use It

- At the start of each workday, to get oriented before opening email or calendar
- When travel, time zone differences, or a packed early schedule make a manual catch-up impractical
- When managing several concurrent client engagements and needing to surface the most time-sensitive items first
- On a scheduled basis, so the briefing is waiting when the user starts their day
- When re-entering after a day out of office and needing a fast summary of what accumulated

### How It Works

The skill gathers five categories of signal in parallel: a web search for the five most relevant industry news stories from the past 24 hours (filtered to the user's role and sector), a scan of the previous day's email and Teams traffic for threads where a direct reply or action is owed, a sweep of client-facing updates across email, Teams, and project sources, a cross-source review of open tasks and commitments, and a calendar pull for today's schedule in the user's time zone.

Distribution-list and broadcast traffic is filtered from the communications and client sections — only threads where the user is a direct, named participant with a specific action owed are included. Calendar events respect privacy: private appointments appear as time blocks, not raw titles. The output is formatted as clean HTML email with labeled section headers and sent to the user's inbox. The skill saves the HTML to an output folder and confirms file existence before reporting the send as complete.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  morning-briefing/
    SKILL.md
```

The front matter requires `name` and `description`. No additional configuration files are required. To run the skill on a daily schedule, configure the trigger in Copilot Cowork's scheduling settings.

### How to Use It

- *"Send my morning briefing."*
- *"Morning brief."*
- *"Brief me."*
- *"Start my day."*
- Runs automatically on a daily schedule when configured.

### Key Design Principles

- **No fabrication.** If a section has no data, the skill says so — it does not fill sections with placeholder language or speculation.
- **DL filtering.** Distribution-list and broadcast traffic is excluded from the communications and client sections. Only direct, actionable threads where the user has a named obligation are surfaced.
- **Privacy-aware calendar.** Private and personal calendar events appear as opaque time blocks to avoid surfacing personal information in a sent email.

---


## 5. Weekday Morning Briefing

### What It Is

The Weekday Morning Briefing is a Chris McNulty-specific start-of-day email optimized for workdays. It expands the general Morning Briefing into an eight-section briefing that adds location-aware weather, Boston sports, and enterprise AI news while preserving the same goal: a fast, factual scan of what matters before the day starts. It is intentionally user-scoped — if anyone other than Chris requests a morning briefing, the general `morning-briefing` skill should be used instead.

### When to Use It

- On a weekday morning, when Chris wants one consolidated read on overnight developments before opening the full inbox
- When travel or calendar context should change the weather and schedule assumptions for the day
- When enterprise AI developments in the last 24 hours need to be surfaced alongside operational priorities
- When a scheduled workday send should arrive as a polished HTML email before the first meeting
- When Monday's briefing needs to roll up the weekend rather than only the prior 24 hours

### How It Works

The skill resolves the day from Pacific Time, extends look-back windows through the weekend on Mondays, and runs the independent lookups in parallel. It assembles eight sections in a fixed order: top news, weather, Boston sports, enterprise AI news, open threads awaiting reply, priority client updates, committed tasks, and today's calendar.

Several gates keep the briefing reliable. Weather is location-aware, inferred from calendar and travel context before falling back to Bothell, Washington. Sports coverage includes only in-season Boston teams, and enterprise AI news must be explicitly date-verified within the last 24 hours or omitted. Communications sections apply a strict addressee test so distribution-list traffic and community aliases are not misrepresented as personal follow-ups owed by Chris.

Delivery is also opinionated: the skill builds a complete HTML document, saves it to `output/`, verifies the artifact exists, and sends the message by passing the saved file path as `body_file_path` with `content_type="HTML"`. That file-based send path is required to preserve Outlook rendering.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  weekday-morning-briefing/
    SKILL.md
```

The front matter includes `name`, `description`, and `cowork` metadata with `category` and `icon` fields. No additional repository files are required, but tenant-specific recipient addresses and branding values must be supplied at runtime.

### How to Use It

- *"Send my weekday morning briefing."*
- *"Give me my weekday briefing."*
- *"Run my weekday briefing."*
- *"Start my workday."*
- *"What did I miss overnight?"*

### Key Design Principles

- **Identity and workday guardrails.** This skill is only for Chris McNulty on a workday start; other users should fall back to the general `morning-briefing` skill.
- **Freshness before fullness.** Weather, sports, and enterprise AI sections prefer omission over stale filler; every AI news item must be date-verified inside the last 24 hours.
- **File-based HTML delivery.** The email body is sent from a saved HTML file, not an inline string, to avoid escaped or flattened rendering in Outlook.

---

## 6. Synozur Board

### What It Is

The Synozur Board skill creates a virtual board of directors — nine distinct personas, each representing a specific domain of expertise — to review, challenge, and vote on any topic the user brings to them. It transforms a single Copilot session into a structured multi-perspective deliberation with independent voices that disagree, a Chair who frames without advocating, and formal voting when requested.

### When to Use It

- Before a major strategic decision, to stress-test reasoning from multiple angles before committing
- When preparing a presentation or proposal and wanting to anticipate the strongest objections
- When a team is converging too quickly and an outside challenge would sharpen the analysis
- When a governance-critical question — succession, capital allocation, ethical dilemma — requires formal deliberation
- When the user wants to hear how specific leadership perspectives (operational, financial, creative, macro) would react to a situation

### How It Works

The skill operates in three modes: **Advisory** (default exploration), **Board** (formal decision with vote), and **Review** (post-decision learning). The user's question is routed to three to five relevant personas plus a mandatory dissent slot — the voice most likely to disagree with emerging consensus.

The nine board members each cover a distinct domain: Buffett (capital and downside), Krugman (macro and mechanism), Cuban (revenue and deal economics), Belichick (execution readiness), Obama (strategic deliberation and coalition), Nadella (platform strategy), Spielberg (narrative and audience), Sandberg (scalability and people systems), and Hillen (strategy discipline and definitional rigor). Each contribution is attributed and substantive — the Chair's framing establishes facts once, and personas react rather than re-narrate.

For governance votes, all nine voices must contribute. Missing contributions are surfaced explicitly rather than inferred or silently dropped. The skill enforces length budgets per persona to keep responses dense rather than padded.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  synozur-board/
    SKILL.md
```

The front matter requires `name` and `description`. No additional configuration files are required.

### How to Use It

- *"Board review of this proposal."*
- *"Take this to the board — yea or nay."*
- *"What does the board think about this acquisition?"*
- *"Get Buffett and Belichick's take on this execution plan."*
- *"Full board vote on whether to proceed."*

### Key Design Principles

- **Independent reasoning, not consensus synthesis.** Each persona contributes in their own voice. The Chair names where they align and diverge — it does not collapse them into a unified recommendation.
- **Dissent is mandatory.** Every routed response includes a dissent slot. If no real counter-case exists, the skill says so explicitly.
- **Governance acts require all nine.** A vote without all nine voices is not a complete board vote — missing contributions are surfaced, not approximated.

---

## 7. Marketing Skills Bundle

### What It Is

The Marketing Skills Bundle (`copilot-cowork-marketing`) is a chained set of ten Copilot Cowork skills covering the full marketing production cycle — from positioning research through content creation, repurposing, SEO/AEO optimization, distribution planning, and performance analysis. Skills pass their output to downstream skills via SharePoint, creating a closed loop that compounds with each cycle. The bundle is grounded in the user's own Microsoft 365 environment throughout.

### When to Use It

- When building or refreshing a go-to-market positioning framework and needing to ground it in real competitive and customer data from M365
- When scaling content production without expanding headcount
- When a content library exists but distribution, repurposing, and performance tracking are ad hoc
- When branded Word and PowerPoint deliverables need to reflect the organization's actual visual identity
- When performance data should feed back into editorial planning rather than sitting in a separate spreadsheet

### How It Works

An onboarding installer (`install-marketing-skills`) runs first — it interviews the user with Work IQ-drafted answers from their M365 data, captures brand colors, fonts, and logos, and writes a `variables.md` and `brand-assets.md` file to a SharePoint location. All nine downstream skills read from `variables.md` so company-specific values are defined once.

The ten skills chain in sequence: `positioning-researcher` outputs a competitive dossier that feeds `content-strategist`'s editorial calendar; `copywriter` drafts from those briefs; `repurposing-engine` converts each long-form piece into eight to ten cross-channel assets; `seo-aeo-optimizer` handles keyword mapping and AI search signals; `distribution-planner` maps each asset to the right channel and timing; and `performance-analyst` closes the loop by feeding pipeline and engagement data back into the editorial calendar. `branded-doc-generator` and `branded-deck-generator` produce publish-ready Word and PowerPoint files from any confirmed draft.

### How to Build It

Drop the entire `copilot-cowork-marketing` folder into Copilot Cowork:

```
cowork/
  copilot-cowork-marketing/
    install-marketing-skills/
    positioning-researcher/
    content-strategist/
    copywriter/
    repurposing-engine/
    seo-aeo-optimizer/
    distribution-planner/
    performance-analyst/
    branded-doc-generator/
    branded-deck-generator/
    variables.md
```

After ingesting the folder, run `install-marketing-skills` first. The installer drafts all required variables from your M365 data and writes the completed `variables.md` and `brand-assets.md`. See the bundle's `README.md` for the full setup checklist and variables reference.

### How to Use It

- *"Install the marketing skills."* (runs the onboarding installer)
- *"Run positioning researcher on [company] and [3 competitors]."*
- *"Build an editorial calendar for the next two weeks."*
- *"Write LinkedIn posts for the first five briefs."*
- *"Repurpose this blog post into LinkedIn, email, and short-form video."*
- *"Convert this draft into a branded deck."*

### Key Design Principles

- **Variables defined once.** All company-specific values live in `variables.md`. The installer writes this file from M365 data — skills do not require manual re-entry of company context.
- **SharePoint as the connective tissue.** Each skill reads its upstream inputs from SharePoint and writes outputs back. The next skill in the chain picks up where the previous one left off.
- **Branded output requires brand assets.** `branded-doc-generator` and `branded-deck-generator` halt if `brand-assets.md` is absent or incomplete. Brand assets must be captured during installation.

---

## 8. Sales Harness Bundle

### What It Is

The Sales Harness Bundle (`sales-harness-bundle`) is a three-agent outbound sales loop built on Copilot Cowork: **Prospector → Composer → Cadence**. Prospector researches leads and writes dossiers. Composer drafts first-touch and follow-up messages in the firm's voice and places every draft in Outlook before any send. Cadence detects sends, detects replies, and queues follow-ups. Every outbound message requires a human click to send — the bundle does not auto-send in v1.

### When to Use It

- When managing an active outbound prospecting pipeline and needing consistent research and drafting across many leads
- When outbound messaging quality varies across reps and a shared voice standard would improve results
- When follow-up cadence is inconsistently applied and leads are falling through the gaps
- When compliance with opt-out detection, send caps, and domain suppression is required at the lead level
- When a firm wants to maintain human review of every outbound message while reducing the manual drafting load

### How It Works

The harness represents each prospect as a markdown file with YAML frontmatter. State transitions drive the loop: `new → researched → draft_pending_approval → sent → awaiting_reply → replied / cadence_step_due / dormant`. Each agent handles a narrow slice of those transitions.

Prospector sweeps leads with `state: new`, runs ICP qualification against the `icp-research` skill's definition and disqualifiers, and writes a research dossier into the prospect file. Composer picks up `state: researched` leads, reads the dossier and the firm's voice DNA, checks compliance (banned phrases, suppression list, send caps), drafts the message to both the markdown file and Outlook Drafts, and transitions the state to `draft_pending_approval`. Cadence scans sent items and the inbox to detect sends and replies, then queues pre-approved follow-up templates as additional drafts. Kill switches enforce hard stops: global pause, opt-out detection, send caps, and a reply-rate floor.

Voice DNA extraction is required before the Composer will draft. The bundle includes `voice-dna-extract.md`, a prompt for extracting the firm's voice from twenty or more past outbound messages that received replies.

### How to Build It

Drop the `sales-harness-bundle/skills/` folder into Copilot Cowork's skills directory and replace the placeholder tokens with your firm's values:

```
cowork/
  sales-harness-bundle/
    skills/
      prospector/
      icp-research/
      composer/
      outbound-voice/
      cadence/
      cadence-rules/
      compliance/
      kill-switches/
      prospect-files/
      outlook-ops/
```

See `sales-harness-bundle/README.md` for the full placeholder list, OneDrive folder setup, voice DNA extraction procedure, and configuration reference.

### How to Use It

- *"Run prospector on all new leads."*
- *"Run composer on researched leads."*
- *"Run cadence."*
- Operator commands `/run-prospector`, `/run-composer`, and `/run-cadence` can be registered as Cowork slash commands.

### Key Design Principles

- **Human in the loop for every send.** Composer creates an Outlook draft. The human reviews and clicks Send. Auto-send is a future configuration flag — not available in v1.
- **Voice is load-bearing.** The Composer refuses to draft until voice DNA has been extracted from real past messages that received replies. Generic AI-voice drafts are explicitly blocked.
- **Kill switches are hard stops.** Send caps, reply-rate floors, opt-out detection, and the global pause flag halt all three agents on every run — they are enforced constraints, not warnings.

---


## 9. Spot Financial Outliers

### What It Is

Spot Financial Outliers reviews a financial statement — income statement, balance sheet, or cash flow — and identifies values that appear materially unusual compared with prior periods, peer line items, or expected financial patterns. It is an analytical review skill, not a modeling, audit, or compliance opinion workflow.

### When to Use It

- When a finance leader wants a fast anomaly review of an uploaded statement before a leadership meeting
- When quarter-over-quarter or year-over-year changes need to be screened for unusual movement
- When a statement has many line items and the user wants help ranking what is worth attention
- When the source file is structured data, commonly Excel, and the goal is interpretation rather than editing formulas
- When a reviewer needs quantified flags without speculating about the root cause

### How It Works

The skill first locates the source statement, typically in `input/`, and reads the data with the xlsx skill so every period column and line item is available for computation. It then identifies the statement type and applies several outlier checks: period-over-period variance, ratio-based anomalies, peer-category inconsistencies, and structural red flags such as unexpected negative values or major swings without obvious offsets elsewhere.

Potential findings are ranked by materiality so the response surfaces only meaningful deviations. The output starts with a short summary, followed by a concise list of flagged line items using the statement's exact labels and quantified deltas where possible. Each finding is framed carefully as a potential anomaly, a likely business-driven change, or an item that needs additional context.

### How to Build It

Place the `SKILL.md` file at:

```
cowork/
  spot-financial-outliers/
    SKILL.md
```

The front matter includes `name`, `description`, and `cowork` metadata with `category` and `icon` fields. The skill depends on the xlsx skill for spreadsheet extraction but does not require additional repository-local configuration.

### How to Use It

- *"Spot outliers in this income statement."*
- *"Review this balance sheet for unusual numbers."*
- *"Do any of these expenses look out of line?"*
- *"Analyze this cash flow statement and highlight anomalies for leadership."*
- *"Check this financial statement for unusual values."*

### Key Design Principles

- **Do the math, don't guess.** Variances, ratios, and structural checks are computed from the statement data rather than inferred by inspection.
- **Materiality over noise.** Small fluctuations are suppressed so attention stays on changes that are meaningfully unusual.
- **Analytical, not authoritative.** The skill flags what deserves follow-up, but it does not issue audit conclusions, accounting judgments, or compliance opinions.

---

# Copilot in SharePoint Skills

---

## 10. Content Expiration Sentinel

### What It Is

SharePoint environments accumulate content over time, and outdated policies, procedures, and reference documents create real risk when people rely on them. The Content Expiration Sentinel scans a SharePoint site or library and identifies content that is likely overdue for review — prioritized by impact and ownership status. It gives content managers and governance leads a clear starting point, rather than a manual audit.

### When to Use It

- When preparing for an annual content governance review and need to know where to focus first
- When a compliance team asks whether policy documents are current before a certification or audit
- When a site has grown organically and no one has a clear picture of what is stale
- When onboarding a new content owner and they need to understand the state of what they're inheriting
- When organizational changes (rebranding, restructuring, regulatory updates) require a targeted sweep of affected content

### How It Works

The skill scans every document and page in the specified site or library, evaluating each item against a set of observable signals: how long ago it was last modified, when it was last accessed (where that metadata is available), whether it has a named owner or author, and whether its title or content suggests it is authoritative material — such as a policy, procedure, or guideline.

Items without explicit review or expiration metadata are assessed comparatively — content that is significantly older than similar items in the same library is treated as a candidate for review. Where formal review metadata exists (such as a "Review Date" column), it is used to refine the assessment alongside those other signals.

Each flagged item is classified into one of three priority tiers: High Priority for content that appears outdated and carries high organizational impact; Medium Priority for content that is possibly stale or has unclear ownership; and Low Priority for older content that appears lower risk. The output is a prioritized report with title, last modified date, owner, and the specific reason for the flag, followed by a summary of patterns — such as clusters of outdated content within a particular team's area or systemic ownership gaps.

The skill does not modify, move, or archive any content. It produces findings only.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  content-expiration-sentinel/
    SKILL.md
```

The front matter requires `name` and `description`. Unlike Cowork skills, SharePoint skills do not use the `cowork` metadata block. The skill is activated by Copilot in SharePoint based on the description and user intent.

### How to Use It

- *"Which documents in this library haven't been updated in over a year?"*
- *"Run a content health check on this site — flag anything that looks outdated."*
- *"Which policies here are missing owners or review dates?"*
- *"Give me a prioritized list of content that needs attention before our audit."*
- *"What's stale in the HR policies library?"*

### Example Output

**Content Expiration Report — HR Policies Library**
*Scan date: May 17, 2026 | Items reviewed: 84 | Items flagged: 19*

**Summary Pattern:** 11 of 19 flagged items are in the Benefits & Compensation folder, with no updates since 2022. Six items lack any named owner.

| Priority | Title | Last Modified | Owner | Reason |
|----------|-------|--------------|-------|--------|
| 🔴 High | Remote Work Policy | Jan 2022 | None | No updates in 3+ years; no owner; policy content |
| 🔴 High | Data Retention Procedure | Mar 2021 | L. Morrison | Referenced in compliance review; not updated since |
| 🟡 Medium | Onboarding Checklist v2 | Aug 2023 | P. Reyes | Newer version (v3) found in same library |
| 🟢 Low | Office Supply Request Form | Sep 2022 | M. Grant | Infrequently used; low impact |

### Key Design Principles

- **Read-only operation.** The skill never modifies, moves, or deletes content under any circumstances — it outputs findings only.
- **Evidence-based classification.** Each flag is tied to a specific, observable signal (age, missing metadata, content type). The skill does not speculate about whether content is incorrect.
- **Patterns over individual items.** The summary section is designed to surface systemic issues — clusters, ownership gaps, structural problems — not just a long list of individual flags.

---

## 11. Onboarding Path Synthesizer

### What It Is

New hires and role transitions are expensive when onboarding is unstructured. The Onboarding Path Synthesizer reads a SharePoint site and assembles a structured, role-specific reading path from the content already there — organized into what a new person needs in their first week, what provides useful background, and what they'll consult on an ongoing basis. It removes the guesswork from onboarding design without requiring a content overhaul.

### When to Use It

- When a new hire is starting and a manager wants to give them a curated starting point rather than a folder full of links
- When someone moves into a new role and needs to get oriented quickly without reading everything
- When a team is scaling and onboarding needs to be consistent without being manually maintained
- When a new vendor, contractor, or partner needs to get up to speed on how the organization works
- When reviewing whether existing onboarding materials are sufficient before a hiring wave

### How It Works

The skill begins by asking for the role's description or key responsibilities if they haven't been provided. It then scans the SharePoint site for relevant content across all available content types — documents, pages, lists, guides, training materials, and policies — using the role description as a relevance filter.

Content is ranked by three factors: relevance to the stated role, recency (recently updated content is favored over older material), and the presence of a clear owner or official designation suggesting it is authoritative. From this ranked set, the skill constructs a three-tier onboarding path: First Week (essential reading, capped at seven items to avoid overwhelm), Background (helpful context for deeper understanding), and Reference (materials to consult when specific situations arise).

Each item in the path includes the document title with a link and a single sentence explaining why it is relevant to this specific role. If the site does not contain enough structured content to build a meaningful path, the skill states this clearly and provides the best available relevant resources instead of padding the output with marginal material. No content is created or modified — the output is guidance only.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  onboarding-path-synthesizer/
    SKILL.md
```

The front matter requires `name` and `description`. The skill is activated by Copilot in SharePoint when a user asks for onboarding guidance, training paths, or reading recommendations for a new hire.

### How to Use It

- *"Build an onboarding path for a new project manager joining next week."*
- *"What should a new HR Business Partner read in their first week?"*
- *"Create a reading list for our new data analyst — focus on what's most important first."*
- *"What training materials on this site are relevant for someone in a client-facing role?"*
- *"Give me a structured onboarding guide I can share with our new team lead."*

### Example Output

**Onboarding Path: Senior Business Analyst**
*Based on content available in the Strategy & Operations site*

---

**First Week** *(Essential — read before day 5)*

1. [How We Work: Operating Principles](link) — Establishes the decision-making norms and communication expectations you'll encounter immediately.
2. [Project Intake & Prioritization Process](link) — Explains how new work is initiated and approved; you'll need this in your first stakeholder meeting.
3. [Stakeholder Directory & Account Map](link) — Identifies key contacts across the business and their areas of accountability.
4. [Analytics Environment Overview](link) — Covers the tools, platforms, and data access procedures relevant to this role.
5. [Q2 Strategy Priorities](link) — Current organizational priorities that will shape your initial workload.

**Background** *(Read in weeks 2–4)*

- [Business Review Cadence & Templates](link) — Context for recurring reporting cycles you'll eventually own.
- [Historical Project Archive: Selected Cases](link) — Useful for understanding how similar problems have been approached in the past.

**Reference** *(Consult as needed)*

- [Data Governance Policy](link) — Required reading before accessing production data.
- [Finance Approval Thresholds](link) — Reference when a project requires budget or procurement involvement.

### Key Design Principles

- **Role-specificity over comprehensiveness.** The path is filtered by the stated role, not a dump of everything on the site. Seven items maximum in the First Week tier preserves focus.
- **Recency signals trustworthiness.** Older content that has not been updated is ranked lower, protecting new hires from acting on stale information.
- **Read-only output.** The skill does not create, update, or reorganize any content — it surfaces and structures what already exists.

---

## 12. Process Compliance First-Pass

### What It Is

Most organizations require that certain documents — policies, procedures, contracts, proposals — meet a defined standard before formal review. The Process Compliance First-Pass reads a document against the relevant checklist and identifies exactly what is present, what is missing, and what is ambiguous — so reviewers spend their time on substance rather than scavenger hunts. It is designed to reduce the back-and-forth cycle between authors and review committees.

### When to Use It

- Before submitting a policy document for legal or compliance sign-off, to catch gaps early
- When a project proposal must meet a defined gate criterion and the author wants to self-check before formal review
- When a procurement document requires specific clauses and a first-pass screen is needed before routing
- When a team is producing a new procedure and wants to validate it against the organizational template before publishing
- When a compliance team is managing a high volume of submissions and needs triage assistance to prioritize

### How It Works

The skill begins by identifying the document type, preferring metadata if it is present and falling back to title and content signals if not. It then retrieves the corresponding compliance checklist from the site's Compliance Requirements list or a configured reference source. If neither the document type nor the checklist can be determined, the skill stops and reports that clearly rather than proceeding on assumptions.

For each required element in the checklist, the skill evaluates the document and assigns one of three statuses: Present (clearly included and complete), Absent (missing entirely), or Unclear (partially addressed or lacking sufficient detail). Where the classification is not straightforward, the skill captures a brief excerpt or explanation to make the reasoning transparent.

The output is a structured report showing the document type, an overall summary status (Pass, Flag, or Needs Review), and a table of every required element with its status and supporting notes. Missing elements and ambiguous sections are highlighted. The skill does not rewrite, revise, or update the document — it produces a diagnostic only.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  process-compliance-first-pass/
    SKILL.md
```

The front matter requires `name` and `description`. The skill depends on a configured Compliance Requirements list on the SharePoint site or another configured reference source to retrieve the appropriate checklist for each document type.

### How to Use It

- *"Run a compliance first-pass on this policy draft before I send it for review."*
- *"Does this proposal meet all the required elements for the finance committee?"*
- *"Check this procedure against our standard template and flag anything missing."*
- *"Pre-screen this contract before it goes to legal — identify any gaps."*
- *"Give me a compliance readiness report on this submission."*

### Example Output

**Process Compliance First-Pass Report**

- **Document:** Data Retention Policy — Draft v2
- **Document Type:** Information Governance Policy
- **Overall Status:** 🟡 Flag — 2 required elements absent, 1 unclear

| Element | Status | Notes |
|---------|--------|-------|
| Purpose Statement | ✅ Present | Clearly stated in Section 1 |
| Scope Definition | ✅ Present | Covers all enterprise data types |
| Retention Schedule (by data class) | ❌ Absent | No schedule found; required per IG-POL-04 |
| Roles & Responsibilities | ⚠️ Unclear | "Data Owner" referenced but not defined in this document |
| Legal Hold Procedure | ❌ Absent | Required for all governance policies; not addressed |
| Review Cycle | ✅ Present | Annual review, effective date noted |
| Approval Authority | ✅ Present | Signed by CIO |

**Highlighted Gaps:** The Retention Schedule and Legal Hold Procedure are required elements that must be added before this document is eligible for formal sign-off.

### Key Design Principles

- **Checklist-driven, not interpretive.** The skill evaluates only against the elements defined in the relevant compliance checklist — it does not introduce subjective quality assessments.
- **Transparent reasoning.** Every "Absent" or "Unclear" classification includes a note explaining the basis, so authors know exactly what is needed, not just that something is wrong.
- **Read-only, always.** The skill never modifies or rewrites the document. Its role is diagnostic; remediation is the author's responsibility.

---

## 13. Excel to Branded HTML Dashboard

### What It Is

The Excel to Branded HTML Dashboard skill converts an Excel workbook stored in SharePoint into a self-contained, branded HTML dashboard that surfaces key data, tables, and summaries as readable web content. It removes the dependency on Excel for stakeholders who need to view data without installing or opening the application, and produces a shareable page that reflects the organization's visual identity.

### When to Use It

- When a reporting workbook needs to be shared with stakeholders who should not need Excel to view it
- When a dashboard or summary tab exists in a workbook and needs to be published as a webpage
- When operational or financial data should be accessible as a branded internal page rather than a raw spreadsheet
- When a workbook is marked confidential and a controlled-distribution HTML version is preferred over sharing the source file
- When periodic reports need to be converted to a consistent, scannable format without manual reformatting

### How It Works

The skill begins by locating the specified Excel file in SharePoint, or presenting up to five recent `.xlsx` matches for selection if the file is not specified. It then inspects the workbook structure — listing worksheets, identifying named ranges, Excel Tables, chart objects, and classifying sheets as data dumps or summary/report views.

Content extraction maps workbook elements to dashboard components: single-value KPI cells become headline metric tiles, Excel Tables become sortable HTML tables, named ranges become labeled data blocks, and charts are rendered as descriptive text cards with the chart title and a plain-text data summary. Branding is determined in priority order from sheet tab colors, SharePoint site theme colors, or a clean neutral default if neither is available.

The output is a self-contained HTML file with all CSS inlined, a responsive layout using CSS Grid or Flexbox, a sticky navigation bar with anchor links to each sheet section, and a footer marking the source file name and generation date. Personally identifiable information is flagged before rendering, and content marked confidential receives a visible distribution-warning banner.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  excel-to-html-dashboard/
    SKILL.md
```

The front matter requires `name` and `description`. The skill is activated by Copilot in SharePoint when a user asks to convert, publish, or create a dashboard from an Excel workbook.

### How to Use It

- *"Turn this spreadsheet into a dashboard."*
- *"Publish our Q2 data workbook as a webpage."*
- *"Create an HTML report from this Excel file."*
- *"Make our data visible without Excel."*
- *"Convert this spreadsheet to a branded page."*

### Key Design Principles

- **No data fabrication.** Values not present in the workbook are never inserted. Charts that cannot be reproduced visually are represented as descriptive text cards only.
- **Branding from available signals.** The skill determines branding from tab colors and site theme without prompting the user — custom branding prompts are issued only when the user has explicitly requested them.
- **Confidentiality surfaced, not assumed.** Content marked confidential receives a visible banner warning; the skill does not suppress or block output, but ensures the user is aware before distributing.

---

## 14. Library Destination Advisor

### What It Is

The Library Destination Advisor recommends the most appropriate SharePoint document library for a piece of content when the user is unsure where to upload it. It evaluates all libraries visible to the user against the document's type, intended audience, sensitivity, and lifecycle, and returns a ranked primary recommendation with the required metadata to fill in — rather than leaving the decision to guesswork.

### When to Use It

- When uploading a document to a site with many libraries and the right destination is not obvious
- When a new team member needs guidance on where different content types belong
- When a library structure has evolved and existing naming conventions no longer make destinations self-evident
- When a document carries a sensitivity label and library-level retention or permission alignment matters before upload
- When the user wants to confirm there is no more appropriate destination before creating a new library

### How It Works

The skill first gathers information about the content: file name, type, purpose, intended audience, draft or final status, and any sensitivity indicators. It then retrieves all document libraries visible to the user, collecting each library's name, description, effective permission level for the user, content types, metadata columns, retention and sensitivity labels, and general activity level.

Libraries are scored against five factors: name and description match, content type alignment, metadata fit, audience alignment, and lifecycle or retention fit. The highest-scoring library becomes the primary recommendation. If two libraries tie, the more specific scope is preferred. If no library scores above two of five factors, the skill says so plainly rather than forcing a low-confidence recommendation. Libraries where the user does not have at least Contribute permission are excluded before evaluation.

The output names the recommended library with a one-sentence rationale, lists any required metadata the user will need to fill in, and briefly describes a close alternative with its key trade-off. If the content carries a sensitivity marker, the skill adds a reminder to verify that the target library's permissions and labels are appropriate before uploading.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  library-destination-advisor/
    SKILL.md
```

The front matter requires `name` and `description`. The skill is activated by Copilot in SharePoint when a user asks where to save or upload a document.

### How to Use It

- *"Where should I put this document?"*
- *"Which library should I upload this to?"*
- *"I don't know where this belongs — can you advise?"*
- *"What's the right place to save this policy draft?"*
- *"Find me the best library for this report."*

### Key Design Principles

- **No upload on behalf of the user.** The skill recommends only — it never performs the upload, pre-fills metadata, or creates new libraries without explicit user confirmation.
- **Permission-gated recommendations.** Libraries where the user does not have at least Contribute access are excluded before any scoring or ranking occurs.
- **Honest about poor matches.** When no library scores well, the skill says so directly and describes what kind of library would be appropriate — it does not force a low-confidence recommendation.

---

## 15. SharePoint List Dashboard

### What It Is

The SharePoint List Dashboard skill turns any SharePoint list into a single, self-contained HTML dashboard that reads the list **live** every time it loads. The defining property is that the generated file holds no list data. It carries only schema, layout, styling, and query logic, and fetches every row from the SharePoint REST API at page load using the signed-in viewer's session. The result is an interactive, always-current report — KPI tiles, charts, and a sortable table — styled to match the host site's theme colors, fonts, and logo so it looks like the site owner built it.

### When to Use It

- When a list owner wants a visual, always-current summary of a list rather than a static export that goes stale the moment it is saved
- When a project tracker, request queue, or status list needs KPI tiles and charts embedded on a SharePoint page
- When a report must reflect each viewer's own permissions, showing only the rows that person is allowed to see
- When a dashboard needs to survive a site rename or a move between environments without being rebuilt
- When stakeholders need a point-in-time CSV export from a live view without touching the underlying list

### How It Works

The skill first resolves the list, capturing the site absolute URL, the list title, and the list GUID. It queries by GUID, not title, so a later rename does not break the dashboard. It then reads the list schema — internal and display names, field types, choice options, and lookup or person targets — and this schema is the only list content baked into the file. Before generating anything, it runs the exact query the dashboard will run once, to confirm the fields and expansions are correct and to note the item count, then discards those rows rather than embedding them.

Widgets are chosen by field type: choice and boolean fields become donuts with status filter chips, dates become timelines, numbers become KPI tiles and bar charts, and people and lookups become ranked bars. At runtime the file fetches from the REST API with `credentials: 'same-origin'`, follows the response's OData next-link for pagination (reading whichever of `odata.nextLink` or `@odata.nextLink` the negotiated mode returns), renders progressively, auto-refreshes every five minutes with a manual refresh button, and pauses when the tab is hidden. Every widget renders loading, empty, and error states, and failures are shown to the viewer rather than swallowed. Branding is applied as CSS custom properties read from the site theme, with no hard-coded hex values outside the `:root` fallback block, so the dashboard re-themes itself when the site theme changes.

The output is one self-contained `.html` file with charts drawn in inline SVG or canvas and no CDN dependencies, since the SharePoint embed sandbox blocks most external scripts. It is hosted in the site's `SiteAssets` library — the same site collection as the list — which is what makes the credentialed same-origin fetch work, and surfaced with a File viewer or Embed web part.

### How to Build It

Place the `SKILL.md` file at:

```
sharepoint/
  sharepoint-list-dashboard/
    SKILL.md
```

The front matter requires `name` and `description`. The skill is activated by Copilot in SharePoint when a user asks to build a dashboard, chart, or live report from a SharePoint list.

### How to Use It

- *"Build a dashboard from this list."*
- *"Visualize our project tracker."*
- *"Create a live report for the Requests list."*
- *"Turn my list into charts."*
- *"Make a status dashboard I can put on our site."*

### Key Design Principles

- **The file holds no data.** Schema, layout, and query logic are baked in; every row is fetched live at load time. Build-time data access is used only to discover the schema and validate the query, never to populate the deliverable.
- **Same-origin hosting is a requirement, not a preference.** The dashboard authenticates with the viewer's session cookie, which only works when the file is served from the same site collection as the list — hence hosting in `SiteAssets`.
- **Permissions are enforced per viewer.** Two people opening the same dashboard may legitimately see different row counts. The footer states this so it is not mistaken for a bug.
- **On-brand from the site theme.** Colors, fonts, and logo come from the host site with no hard-coded hex outside the `:root` fallback, so the dashboard stays consistent with the site and re-themes automatically.
- **Threshold-aware and accessible.** Above the 5,000-item list view threshold the skill filters on an indexed column or aggregates by group and surfaces the constraint; output is responsive, keyboard-reachable, and announces refreshes through an `aria-live` region.

---

# Copilot Cowork vs SharePoint Skills

These two skill types serve different purposes, live in different environments, and operate under different governance models.

**Copilot Cowork skills** are personal, conversational, and user-controlled. A person builds a skill in Cowork to extend Copilot's behavior in their own context — analyzing their email, reviewing their workload, preparing for their meetings. These skills travel with the individual. They can be shared with specific colleagues or small teams, but they are not centrally managed. Cowork is where skills are created, tested, and refined in real working conditions before any broader rollout is considered.

**Copilot in SharePoint skills** are organizational and governed. They are attached to a site or library and are available to everyone who accesses that site. A SharePoint skill might screen every document submitted for compliance review, build onboarding paths for every new hire, or surface content health issues for a content governance team. These skills are designed for consistent, repeatable use across a group — not personalized to any individual. Because they operate at scale, they require more deliberate design and appropriate oversight before deployment.

The practical distinction is one of reach and accountability:

| | Cowork Skills | SharePoint Skills |
|---|---|---|
| **Scope** | Personal / individual | Organizational / site-wide |
| **Governance** | User-managed | IT or content owner managed |
| **Primary use** | Individual productivity | Operational and governance workflows |
| **Sharing** | Optional, person-to-person | Available to all site users |
| **Where to start** | Build and test here first | Deploy after Cowork validation |

---

# How Skills Scale in an Organization

Skills do not need to be built from scratch at the organizational level. The most effective pattern is a three-stage lifecycle that moves from individual experimentation to governed scale.

**Stage 1: Build and test in Copilot Cowork**
A practitioner — a consultant, analyst, operations lead, or anyone who works with Copilot regularly — identifies a repeated task where a custom skill would save time or improve consistency. They build it in Cowork and use it themselves. Because Cowork operates on real personal data (email, calendar, Teams), testing happens in genuine working conditions, not a sandbox. Problems surface quickly.

**Stage 2: Refine and share with individuals or teams**
Once the skill works reliably, the builder shares it with colleagues or a small team. This phase surfaces edge cases, naming issues, and workflow variations that a single user would never encounter alone. The skill is refined based on real feedback. At this stage, the skill is still informal — it's a shared tool, not a managed asset.

**Stage 3: Promote to SharePoint for governed reuse**
When a skill has proven its value across a group, it becomes a candidate for organizational deployment — typically via a SharePoint site where it can serve everyone who accesses that environment. At this stage, responsibility shifts: the skill needs a named owner, documentation, and alignment with any relevant compliance or data governance requirements. It becomes part of the organization's AI operating model rather than an individual's toolkit.

**Leadership implications**

For leaders and operating model designers, this lifecycle has two important implications. First, the Cowork layer is where organizational AI capability actually develops — through experimentation by practitioners close to the work. Creating space for that experimentation, and establishing a lightweight path from personal skill to governed deployment, is more valuable than top-down skill design. Second, skills that reach SharePoint represent institutional knowledge made actionable. When a compliance checklist or an onboarding framework is embedded in a skill, it stops being a document people might not read and becomes something Copilot actively applies. That shift — from passive artifact to active capability — is the real productivity gain at scale.
