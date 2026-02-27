---
name: financial-dashboard
description: >
  Build a comprehensive financial performance dashboard for any public company using earnings
  reports, SEC filings (10-Q/10-K), and investor relations data. Outputs Excel (.xlsx) or
  interactive HTML for Chrome/browser — or both. ALWAYS use for: "financial dashboard for X",
  "earnings visualization", "quarterly revenue chart", "dashboard like IBM/Nvidia", "10-K
  dashboard", "interactive earnings", "plot margins", or any request combining a company name
  with financial metrics (revenue, net income, margins, EPS, costs) and a time period.
---

# Financial Performance Visualization Dashboard Skill

This skill guides the creation of a **professional financial dashboard** for any publicly
traded company, built entirely from publicly available financial data (Investor Relations
press releases, SEC filings, Yahoo Finance, etc.).

Two output formats are supported — choose based on user request:
- **Excel (.xlsx)** — multi-sheet workbook with charts; ideal for analysis, sharing, and offline use
- **HTML (browser)** — interactive single-file dashboard with Chart.js; ideal for Chrome/browser viewing, dark-mode UI, tab navigation, and filtering

When the user says "for Chrome", "for the browser", or "interactive", produce the **HTML** version.
When the user says "spreadsheet", "Excel", or "model", produce the **Excel** version.
When unclear, **produce both**.

## When to Use This Skill

Use whenever the user asks to:
- Create a financial/earnings dashboard for a company
- Visualize quarterly or annual revenue, earnings, costs, or margins
- Compare financial performance across time periods
- Build an Excel or browser-based financial model from public sources
- Replicate a dashboard previously created for another company

---

## Step-by-Step Workflow

### Step 1 — Clarify Requirements (if not already clear)

Before starting, confirm:
1. **Company name and ticker** (e.g., Apple / AAPL)
2. **Time range** — which years/quarters? (e.g., FY2022–FY2025)
3. **Fiscal year note** — does the company's fiscal year align with the calendar year? If not,
   note the offset (e.g., NVIDIA's FY ends in late January).
4. **Metrics desired** — Revenue, Gross Profit, Operating Income, Net Income, Margins, EPS,
   Segments? Default to all standard income statement metrics.
5. **Output format** — Excel dashboard (default) or other?

If the user says "like the one for IBM/Nvidia" or similar, replicate the structure from the
conversation history.

---

### Step 2 — Data Research

Collect quarterly financial data from multiple public sources. **Always triangulate** across
at least 2–3 sources to catch errors.

#### Primary Sources (in order of preference)

| Source | What to Get | How |
|--------|-------------|-----|
| **Company IR Press Releases** | Exact GAAP quarterly figures | WebSearch or WebFetch from `investor.[company].com` |
| **SEC EDGAR** | 10-Q (quarterly) and 10-K (annual) filings | Search `site:sec.gov [company] 10-Q [quarter]` |
| **Yahoo Finance** | Annual income statement, TTM data | WebFetch `https://finance.yahoo.com/quote/[TICKER]/financials/` |
| **Stockanalysis.com** | Quarterly income statement table | WebFetch `https://stockanalysis.com/stocks/[ticker]/financials/?p=quarterly` |
| **Macrotrends** | Historical quarterly charts | WebSearch `macrotrends [company] revenue quarterly` |

#### Key Metrics to Collect Per Quarter
- **Revenue** (Net Revenue / Total Revenue)
- **Cost of Revenue** (COGS)
- **Gross Profit** = Revenue − Cost of Revenue
- **Gross Margin %** = Gross Profit / Revenue
- **Operating Expenses** (R&D + SG&A)
- **Operating Income** (EBIT)
- **Net Income** (GAAP)
- **Net Margin %** = Net Income / Revenue
- **Diluted EPS**
- **YoY Revenue Growth %**
- **Segment revenue** (if available)

#### Research Tips
- Search for press releases by quarter: `"[Company] announces financial results [Q1/Q2/Q3/Q4] fiscal [year]"`
- For fiscal year offsets, carefully map fiscal quarters to calendar periods
- If exact cost of revenue isn't available per quarter, derive from reported gross margin: `CoR = Revenue × (1 − Gross Margin %)`
- Note any one-time charges or non-GAAP adjustments in the Sources sheet
- For Q4/full-year releases, press releases typically contain full-year income statement tables

---

### Step 3 — Build the Dashboard

Determine the output format (see intro). Then build accordingly.

**For Excel:** Read `references/excel-dashboard-template.md` for full formatting spec.
**For HTML:** Read `references/html-dashboard-template.md` for full HTML/CSS/JS spec.

#### Excel Dashboard — 5 sheets (minimum)

##### Sheet 1 — Dashboard (first tab, set as active)
- Title banner with company name, branding colors, time range
- KPI cards row: highlight key metrics (latest quarter revenue, annual revenue, YoY growth, net margin, gross margin)
- 4 charts (see Chart Specifications below)

##### Sheet 2 — Quarterly Income Statement
- All quarters in columns (or rows), metrics as rows (or columns)
- FY section headers with distinct colors per fiscal year
- Alternating row shading
- YoY growth column with conditional color (green = positive, red = negative)
- Freeze panes on header row

##### Sheet 3 — Annual Summary
- Rolled-up annual totals for all metrics
- YoY delta columns for each consecutive year pair
- 3-year CAGR column
- Commentary column explaining key drivers

##### Sheet 4 — Segment Analysis (if segment data available)
- Revenue breakdown by business segment per quarter
- Segment-as-% of total column
- Stacked bar chart

##### Sheet 5 — Sources & Notes
- One row per data source with: Period, Source name, URL/Reference
- Methodology notes (fiscal year definition, one-time items, rounding)
- Color convention legend (blue = inputs, black = formulas, green = links)

---

#### HTML Dashboard — Structure

For the HTML version, produce a **single self-contained `.html` file** with all CSS and JS inline.
Read `references/html-dashboard-template.md` for the complete spec. Key points:

- **Library**: Use Chart.js 4.x from `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js`
- **Theme**: Dark background (`#0d0d0d`), company brand color as accent, professional card-based layout
- **Navigation**: Tab bar at top — Overview · Quarterly Detail · Annual Summary · Segments · Margins · Sources
- **KPI Cards**: 5 cards at the top of Overview tab — latest revenue, net income, quarterly revenue, gross margin, CAGR
- **Charts per tab**:
  - **Overview**: 2×2 grid — stacked revenue bar, net income line, dual margin lines, annual revenue bar
  - **Quarterly Detail**: filter buttons by FY + full data table + full-width revenue bar
  - **Annual Summary**: FY cards + grouped bar + YoY growth bar + summary table
  - **Segments**: stacked segment bar + DC% line chart + segment table with mini progress bars
  - **Margins & Profitability**: 3-column margin charts + full-width cost structure bar
  - **Sources & Notes**: source cards + notes table
- **Interactivity**: Chart.js hover tooltips, tab switching, FY filter buttons on table
- **All data embedded in JS** — no external data fetches; dashboard works fully offline after download
- **Filename**: `[COMPANY]_Financial_Dashboard.html`

### Chart Specifications

Create these 4 charts on the Dashboard sheet:

| # | Chart Type | Data | Placement |
|---|-----------|------|-----------|
| 1 | Stacked column | Revenue = CoR (bottom) + Gross Profit (top) | Upper-left |
| 2 | Smooth line | Net Income by quarter | Upper-right |
| 3 | Dual line | Gross Margin % and Net Margin % | Lower-left |
| 4 | Column | Annual Revenue by fiscal year | Lower-right |

For Chart 1, use company brand color for Gross Profit and a muted tone for CoR.

---

### Step 4 — Formatting Standards (Excel)

*These apply to Excel output only. For HTML, see `references/html-dashboard-template.md`.*

Follow financial modeling color conventions:
- **Blue text** (`RGB 0,0,255`) — hardcoded input values sourced from external data
- **Black text** — Excel formulas and calculated values
- **Green text** — cross-sheet formula references
- **Yellow background** — cells requiring attention or estimates

Number formats:
- Currency in millions: `#,##0;(#,##0);"-"` (use `$M` in column headers)
- Percentages: `0.0%;(0.0%);"-"`
- Negative numbers: parentheses notation `(123)`, not minus sign

Headers:
- Title row: 14pt bold, white on dark branded color, merged, centered
- Column headers: 10pt bold, white on medium blue (`#2E75B6`)
- FY section headers: 10pt bold, white on FY-specific color

Sheet tab colors: use distinct colors per sheet for easy navigation.

---

### Step 5 — Verify

**Excel:** Always run after saving:
```bash
python scripts/recalc.py <output_file>.xlsx 60
```
Check `status` is `"success"` and `total_errors` is `0`. Fix any formula errors before delivering.

**HTML:** Open the file in a browser (or use the browser tool) and verify:
- All 6 tabs load without JavaScript errors
- Charts render with correct data
- KPI cards show correct values
- Table filter buttons work
- Tooltips appear on chart hover

---

### Step 6 — Deliver

**Excel:** Save to `/sessions/.../mnt/outputs/[COMPANY]_Financial_Dashboard_[FY_RANGE].xlsx`

**HTML:** Save to `/sessions/.../mnt/outputs/[COMPANY]_Financial_Dashboard.html`

Provide a `computer://` link for each file. Include a brief 2–3 sentence summary of:
quarters covered, key metrics visible, and major financial highlights in the data.

---

## Company-Specific Notes

### Tech Companies with Non-Calendar Fiscal Years
| Company | FY End | Note |
|---------|--------|------|
| NVIDIA | Late January | FY2026 = Feb 2025 – Jan 2026 |
| Apple | Late September | FY2025 = Oct 2024 – Sep 2025 |
| Microsoft | Late June | FY2025 = Jul 2024 – Jun 2025 |
| Salesforce | Late January | Same as NVIDIA offset |

### Data Availability
- Most companies have full quarterly data for the last 3–4 years readily available
- SEC EDGAR has 10-Q filings going back to the 1990s for large-cap companies
- For very recent quarters (last 60 days), check the IR website directly

---

## Handling Estimation

When exact figures are unavailable (e.g., cost of revenue not separately disclosed):
1. Use reported gross margin % to derive CoR: `CoR = Revenue × (1 − GM%)`
2. Mark estimated cells with a light yellow fill and add a note in Sources
3. State clearly in the Sources sheet: "Estimated from reported gross margin %"

Never present estimates as exact reported figures without labeling them.

---

## Quality Checklist

Before delivering, verify:
- [ ] All 16 quarters populated (or however many are in scope)
- [ ] Annual totals match sum of quarterly figures (within rounding)
- [ ] YoY growth formulas reference correct prior-year quarter
- [ ] All charts display correct data ranges
- [ ] Zero formula errors (recalc.py confirms)
- [ ] Sources sheet complete with URLs for each data point
- [ ] Fiscal year definition clearly stated
- [ ] One-time items flagged and explained (e.g., NVIDIA H20 charge Q1 FY26)
- [ ] Sheet tabs colored and named clearly
- [ ] File saved to outputs folder with descriptive filename
