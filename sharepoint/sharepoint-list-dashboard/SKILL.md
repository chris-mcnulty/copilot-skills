---
name: SharePoint List Dashboard
description: Generates a dynamic, self-contained HTML dashboard that reads a SharePoint list live at render time via the REST API, styled to match the host site's branding (theme colors, fonts, logo). Use when the user asks to "build a dashboard from this list", "visualize this SharePoint list", "create a live report for [list]", "turn my list into charts", "make a status dashboard", or references a SharePoint list URL and wants a self-refreshing visual summary.
---

# SharePoint List Dashboard Builder

Turn any SharePoint list into a single-file, interactive HTML dashboard that
queries the list **live** every time it loads — no stale snapshots — and looks
like it was built by the site owner.

## When to use

Trigger on: "dashboard from my list", "visualize this list", "chart my project
tracker", "build a live report from [list name]", "show list metrics", or any
request that pairs a SharePoint list with words like dashboard, report, chart,
KPI, metrics, live, or visualize.

Do **not** use for: static Word/PowerPoint summaries, Power BI semantic models,
or lists with fewer than 3 items (answer inline instead).

## Core principle: the file holds no data

The generated HTML contains **schema, layout, styling, and query logic only**.
Every row it displays is fetched from the list at page load. Build-time data
access is used solely to discover the schema and validate the query — never to
populate the deliverable.

## Workflow

### 1. Resolve the list
- If given a URL, parse the site path and list name from it.
- Otherwise resolve the site, then enumerate lists and match by title.
- Confirm the match if two or more lists have similar names; otherwise proceed.
- Never guess a list ID or site ID — always resolve from a lookup.
- Capture and record for the generated file: **site absolute URL**, **list
  title**, **list GUID**. The GUID is the stable identifier — a renamed list
  breaks a title-based query, so query by GUID and keep the title for display.

### 2. Read the schema before anything else
Pull the list's columns. Capture for each field:

| Property | Why it matters |
|---|---|
| Internal name | Used in `$select` and as the runtime data key |
| Display name | Used as the column header and axis label |
| Type | Drives the widget choice (see §4) |
| Choice options | Becomes the filter/legend set, rendered before data arrives |
| Lookup/person target | Determines the `$expand` clause |
| Required / indexed | Indexed fields are safe to filter on above 5,000 items |

Skip system fields (`ContentTypeId`, `_UIVersionString`, `Attachments`,
`GUID`, `owshiddenversion`, `ComplianceAssetId`, `AppAuthor`, `AppEditor`).

The schema is the **only** list content that gets baked into the file.

### 3. Validate the query at build time
Run the exact query the dashboard will run, once, to confirm it succeeds and
the field names are correct. Use the result to sanity-check widget choices —
then discard it. Do not embed the returned rows.

Note the item count so you can warn the user about threshold limits, and
confirm lookup/person expansions return the display values you expect.

### 4. Choose widgets by field type

| Field type | Default widget |
|---|---|
| Choice / Boolean | Donut + status filter chips |
| Date / DateTime | Timeline or trend line, grouped by month |
| Number / Currency | KPI tile (sum, avg, min/max) + bar chart |
| Person or Group | Horizontal bar, top 10 by count |
| Lookup | Grouped bar |
| Single line of text | Sortable table column only |
| Multiline / Rich text | Table detail on row expand |
| Managed metadata | Tag cloud or grouped bar |

Standard layout, top to bottom:
1. **Header** — site logo, list title, live item count, "last refreshed" clock
2. **KPI row** — 3–5 tiles from numeric and status fields
3. **Chart grid** — 2 columns desktop, 1 column mobile
4. **Data table** — sortable, searchable, paginated at 25 rows
5. **Footer** — source list link, refresh interval, permissions note

Every widget must render three states: **loading** (skeleton), **empty**
(explanatory text, not a blank frame), and **error** (message + retry button).
Chip sets and axis labels come from the schema, so they render correctly during
the loading state before any row arrives.

### 5. Fetch data at runtime

The dashboard calls the SharePoint REST API from the browser, using the
signed-in user's session:

```js
const ENDPOINT =
  `${SITE_URL}/_api/web/lists(guid'${LIST_ID}')/items` +
  `?$select=${SELECT_FIELDS}` +
  `&$expand=${EXPAND_FIELDS}` +
  `&$orderby=Modified desc` +
  `&$top=2000`;

const res = await fetch(ENDPOINT, {
  method: 'GET',
  credentials: 'same-origin',
  headers: { 'Accept': 'application/json;odata=nometadata' }
});
```

Runtime rules:
- **Same-origin only.** `credentials: 'same-origin'` carries the user's auth
  cookie. This works when the file is served from the same site collection as
  the list — which is why §8 requires hosting it in `SiteAssets`. Cross-site
  hosting will fail auth; do not attempt to work around it with tokens.
- **Follow pagination.** Loop on the `odata.nextLink` in each response until it
  is absent. Render progressively — update charts after each page so a large
  list shows partial results immediately rather than a long blank wait.
- **Derive site URL from context, don't hard-code it.** Read
  `_spPageContextInfo.webAbsoluteUrl` when present so the dashboard survives a
  site rename or a move between environments; fall back to the resolved URL
  captured in §1 only if that global is unavailable.
- **Refresh:** auto-refresh every 5 minutes, plus a manual refresh button. Show
  a "last refreshed HH:MM" stamp that updates on every successful fetch. Pause
  the timer when the tab is hidden (`visibilitychange`) so a backgrounded page
  doesn't hammer the API.
- **Aggregate in the browser**, not at build time — all KPI sums, groupings,
  and counts compute from the freshly fetched array on every refresh.
- **Errors are visible, never silent.** A 403 renders "You don't have access to
  this list"; a 404 renders "List not found — it may have been deleted or
  renamed"; a threshold error renders the throttling guidance from §9. Keep the
  last good dataset on screen when a refresh fails, and flag the stamp as stale.

### 6. Match the site branding

Pull the host site's theme and apply it as CSS custom properties. Map:

```css
:root {
  --brand-primary:   /* site theme primary */
  --brand-dark:      /* themeDark — headers, active states */
  --brand-light:     /* themeLighter — chart fills, hover rows */
  --neutral-fg:      /* body text */
  --neutral-bg:      /* page background */
  --brand-font:      /* site font, fallback: "Segoe UI", system-ui, sans-serif */
}
```

Rules:
- Every chart series, tile accent, and table header pulls from these variables —
  no hard-coded hex values anywhere in the output.
- Prefer reading theme values live from `_spPageContextInfo.themedCssFolderUrl`
  or the page's theme variables so the dashboard re-themes itself when the site
  theme changes; write the build-time values into `:root` as the fallback.
- Use the site logo from the site's header assets. If unavailable, render the
  list title as a text wordmark in `--brand-primary`. Never substitute a
  placeholder image.
- Derive a 5-step categorical palette from the primary by rotating hue while
  holding saturation and lightness, so multi-series charts stay on-brand.
- Respect contrast: if primary is darker than #767676, use white text on it;
  otherwise use `--neutral-fg`.
- If theme colors can't be read, fall back to SharePoint default blue
  (`#0078d4`) and say so in your reply.

### 7. Build the file

Produce **one self-contained `.html` file** — no build step, no external CSS.

- Charts drawn with inline SVG or `<canvas>` and vanilla JS. No CDN links,
  no npm packages — SharePoint's embed sandbox blocks most external script
  sources, and anything depending on a CDN chart library renders blank.
- Interactivity required: filter chips, column sort, free-text search, and a
  date-range selector when a date field exists. Filters apply client-side to the
  fetched array and **survive a refresh** — reapply the active filter set after
  each fetch rather than resetting the view.
- Responsive: CSS grid that collapses to a single column under 768px.
- Accessible: semantic table markup, `aria-label` on every chart, keyboard-
  reachable controls, a visually-hidden data table behind each chart, and an
  `aria-live="polite"` region announcing "Data refreshed, N items".
- Include an "Export current view to CSV" button that serializes the live,
  filtered array — this is how users get a point-in-time copy.

Save the finished file to the deliverables folder and name it
`<ListName>-dashboard.html`.

### 8. Verify before you report
- Confirm the file exists in the output folder.
- Confirm **no list rows are embedded** — search the file for values you saw in
  the build-time query. Finding any means the snapshot leaked in; strip it.
- Confirm the endpoint string contains the correct list GUID and every
  `$select` field matches an internal name from §2.
- Confirm loading, empty, and error states each render without a data array
  present.
- Confirm no `#` hex literals remain outside the `:root` block.

## Embedding in a SharePoint page

Upload the file to the site's **`SiteAssets`** library — same site collection as
the list — then surface it with a **File viewer** web part, or an **Embed** web
part pointing at the file URL.

Same-collection hosting is not a style preference: it is what makes the
same-origin credentialed fetch work. Custom script must be enabled on the site
for the Embed path.

## Guardrails

- **Never fabricate list data.** The file ships with zero rows; if a fetched
  field is empty, render it as empty.
- **Permissions are enforced by SharePoint, per viewer.** Two people opening the
  same dashboard may legitimately see different row counts. State this in the
  footer — do not treat it as a bug.
- **Don't render person email addresses** unless the user explicitly asks;
  display names only.
- **Above 5,000 items**, the list view threshold applies: filter on an indexed
  column in the `$filter` clause, or aggregate by group. Tell the user which
  strategy you used and surface the constraint in the footer.
- Throttle defensively — on a 429 or 503, back off exponentially and show the
  retry countdown rather than looping.
- If a column type isn't in the §4 table, put it in the table and skip charting
  it rather than guessing a visualization.
