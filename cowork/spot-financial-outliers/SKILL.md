---
name: spot-financial-outliers
description: |
  Reviews a financial statement (Income Statement, Balance Sheet, or Cash Flow)
  and identifies values that appear materially unusual compared to historical
  periods, peer line items, or expected financial patterns. Use when user asks
  to "spot outliers", "spot anomalies", "review this financial statement",
  "check for unusual numbers", "analyze this income statement", "do any of
  these expenses look out of line", "highlight anomalies for leadership", or
  references an attached Income Statement, Balance Sheet, or Cash Flow file
  and asks for analytical review.
  Do NOT use for: routine spreadsheet edits, formula creation, financial
  modeling, budget building, or audit/compliance opinions — this skill is
  for analytical review only.
cowork:
  category: analysis
  icon: DataPie
---

# Spot Outlier Values in a Financial Statement

Identifies values in a financial statement that appear materially unusual
compared to historical periods, peer line items, or expected financial
patterns, and explains why those values may warrant attention.

## When to Use

- The file is a financial statement (Income Statement, Balance Sheet, or Cash Flow)
- The data is structured in a table (commonly Excel)
- The user asks to "review", "analyze", "check", or "spot anomalies or outliers"
- The purpose is analytical review, not restatement of results

## When NOT to Use

- Building or editing financial models, budgets, or forecasts
- Creating new spreadsheets or adding formulas — route to the xlsx skill
- Audit opinions, compliance reviews, or accounting restatements
- General "summarize this file" requests with no analytical intent

## Workflow

1. **Locate the file.** Check `input/` first for uploaded statements. Use Glob
   with `input/**/*.xlsx` or similar. If no file is found, ask the user to
   share one.
2. **Read the statement.** Use the xlsx skill's reader to extract the data,
   including all time-based columns (months, quarters, years) and line items.
3. **Identify the statement type.** Income Statement, Balance Sheet, or Cash
   Flow — this shapes what "normal" looks like.
4. **Apply outlier detection** (see principles below). Compute period-over-
   period variances, ratios, and structural checks with code — never estimate
   arithmetic.
5. **Rank by materiality.** Surface only items where the magnitude or
   percentage change is meaningful. Suppress noise.
6. **Write the response.** Summary paragraph first, then a short list of
   flagged items with reasoning and quantified deltas.

## Outlier Detection Principles

Flag values that meet **one or more** of these conditions:

- **Large variance from prior periods** — sudden increases or decreases
  inconsistent with trend
- **Ratio-based anomalies** — expenses disproportionate to revenue; margins
  deviating significantly from historical norms
- **Category inconsistencies** — a single line item behaving differently than
  peer accounts
- **Structural red flags** — negative values where not typically expected;
  material swings without offsetting movements elsewhere

## Interpretation Rules

- Do not assume accounting errors
- Clearly distinguish:
  - "Potential anomaly"
  - "Likely business-driven change"
  - "Requires additional context"
- Use plain language and explain *why* something stands out
- Prioritize materiality over noise

## Presentation Standards

- Summarize first, then list details
- Reference line item names exactly as shown in the statement
- Quantify the difference when possible (percentage or magnitude)
- Avoid speculative conclusions

## Governance

- Supports review and insight, not audit opinions
- No financial advice or compliance judgments
- Final interpretation remains with finance leadership

## Example Output Style

- "Marketing spend increased 42% quarter-over-quarter while revenue grew 6%,
  which is a notable deviation from prior trend."
- "Accounts receivable declined sharply without a corresponding cash increase,
  which may warrant follow-up."
