# HTML Financial Dashboard — Template Reference

## Overview

The HTML dashboard is a **single self-contained file** with all CSS, JS, and data inline.
It uses Chart.js 4.x for charts, a dark theme with company brand accents, and tab-based
navigation. No server or internet required once downloaded.

---

## File Structure

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Chart.js from cdnjs -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>/* All CSS inline */</style>
</head>
<body>
  <!-- 1. Top header (sticky) -->
  <!-- 2. Nav tab bar -->
  <!-- 3. Page sections (one per tab, display:none until active) -->
  <script>/* All data + chart init + nav logic */</script>
</body>
</html>
```

---

## CSS Variables (customize per company)

```css
:root {
  --brand-primary: #76b900;   /* Company brand color — replace per company */
  --brand-dark:    #5a8c00;
  --bg-dark:       #0d0d0d;   /* Page background */
  --bg-card:       #1a1a1a;   /* Card / panel background */
  --bg-card2:      #222222;
  --bg-table-head: #1f1f1f;
  --border:        #2e2e2e;
  --text-primary:  #f0f0f0;
  --text-secondary:#a0a0a0;
  --text-muted:    #666;
  /* Semantic colors */
  --positive:      #2ecc71;
  --negative:      #e74c3c;
  --blue:          #4a90d9;
  --orange:        #e07b39;
  --purple:        #9b59b6;
  --teal:          #1abc9c;
}
```

## Company Brand Colors

| Company  | Primary Color | Usage in `--brand-primary` |
|----------|---------------|---------------------------|
| NVIDIA   | `#76b900`     | NVIDIA green |
| Apple    | `#555555`     | Apple dark gray |
| Microsoft| `#00a4ef`     | MS blue |
| IBM      | `#006699`     | IBM blue |
| Google   | `#4285f4`     | Google blue |
| Amazon   | `#ff9900`     | Amazon orange |
| Meta     | `#0866ff`     | Meta blue |

---

## Top Header

```html
<div class="top-header">
  <!-- Left: logo square + title + subtitle -->
  <!-- Right: ticker badge + source note -->
</div>
```

- Sticky (`position: sticky; top: 0; z-index: 100`)
- Height: 70px
- Background: dark gradient with brand color tint
- Bottom border: 2px solid brand color
- Logo: 40×40px colored square with company initials

---

## Navigation Tabs

```html
<div class="nav-tabs">
  <div class="nav-tab active" onclick="showPage('overview')">📊 Overview</div>
  <div class="nav-tab" onclick="showPage('quarterly')">📅 Quarterly Detail</div>
  <div class="nav-tab" onclick="showPage('annual')">📆 Annual Summary</div>
  <div class="nav-tab" onclick="showPage('segments')">🏢 Segments</div>
  <div class="nav-tab" onclick="showPage('margins')">📈 Margins & Profitability</div>
  <div class="nav-tab" onclick="showPage('sources')">🔗 Sources & Notes</div>
</div>
```

Tab switching JS:
```js
function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + id).classList.add('active');
  event.target.classList.add('active');
}
```

---

## KPI Cards (Overview Tab)

5 cards in a CSS grid row. Each card:
```html
<div class="kpi-card" style="--accent-color:#76b900">
  <div class="kpi-label">FY2026 Revenue</div>
  <div class="kpi-value">$216B</div>
  <div class="kpi-change pos">▲ +65% YoY</div>
  <div class="kpi-sub">vs $130.5B in FY2025</div>
</div>
```

Standard KPI order:
1. Full-year revenue (latest FY) — brand color accent
2. Full-year net income (latest FY) — blue accent
3. Most recent quarter revenue — orange accent
4. Gross margin % (latest quarter) — teal accent
5. Revenue CAGR (full period) — purple accent

---

## Chart Configuration

### Global Chart.js Defaults

```js
Chart.defaults.color = '#a0a0a0';
Chart.defaults.borderColor = '#2e2e2e';
Chart.defaults.font.family = "'Segoe UI', sans-serif";
Chart.defaults.font.size = 11;

// Reusable tooltip config
const tooltipPlugin = {
  backgroundColor: '#1a1a1a',
  borderColor: '#444',
  borderWidth: 1,
  titleColor: '#f0f0f0',
  bodyColor: '#a0a0a0',
  padding: 10,
  cornerRadius: 6,
};
```

### Standard 4-Chart Overview Grid

| Position | Type | Data | Colors |
|----------|------|------|--------|
| Upper-left | Stacked bar | Revenue = CoR + Gross Profit | CoR: orange 40% opacity, GP: brand 60% |
| Upper-right | Line (filled) | Net Income by quarter | Blue, filled area |
| Lower-left | Dual line | Gross Margin % + Net Margin % | Brand + blue |
| Lower-right | Bar | Annual Revenue by FY | FY-specific colors |

### FY Color Mapping (for bar chart background variety)

```js
const FY_COLORS = {
  FY2021: '#7030a0',
  FY2022: '#c55a11',
  FY2023: '#e07b39',
  FY2024: '#4a90d9',
  FY2025: '#2ecc71',
  FY2026: '#76b900',  // or brand color for most recent
};
```

Use `FY_COLORS[q.fy] + 'aa'` for 67% opacity backgrounds.

---

## Data Arrays

Embed all data directly in JavaScript — no API calls:

```js
const quarters = [
  // [label, periodEnd, rev, cor, gp, opex, opInc, netInc, gm%, nm%, fy]
  ["Q1 FY23", "May 1, 2022", 8290, 2862, 5428, 3280, 2148, 1618, 65.5, 19.5, "FY2023"],
  // ... all 16 quarters
];

const annual = [
  // [fy, rev, cor, gp, opex, opInc, netInc, gm%, nm%]
  ["FY2023", 26974, 11619, 15355, 11346, 4010, 4368, 56.9, 16.2],
  // ...
];

const segData = [
  // [quarter, dc, gaming, profviz, auto, oem, total]
  ["Q1 FY23", 3750, 3620, 622, 138, 160, 8290],
  // ...
];
```

---

## Interactive Table Filter (Quarterly Tab)

```html
<div class="filter-bar">
  <button class="filter-btn active" onclick="filterTable('all', this)">All Years</button>
  <button class="filter-btn" onclick="filterTable('FY2023', this)">FY2023</button>
  <!-- etc -->
</div>
```

```js
function filterTable(fy, btn) {
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  document.querySelectorAll('#quarterlyTable tbody tr').forEach(tr => {
    if (fy === 'all' || tr.dataset.fy === fy || tr.classList.contains('fy-row')) {
      tr.style.display = '';
    } else {
      tr.style.display = 'none';
    }
  });
}
```

---

## Segment Progress Bars

Use inline HTML in table cells for visual segment percentage bars:

```html
<td>
  <div class="seg-bar-wrap">
    <div class="seg-bar-bg">
      <div class="seg-bar-fill" style="width:${dcPct}%;background:${color}"></div>
    </div>
    <span class="seg-pct">${dcPct.toFixed(1)}%</span>
  </div>
</td>
```

---

## Key CSS Patterns

### Card style
```css
.chart-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 20px;
}
```

### FY section row in tables
```css
.fy-row td {
  background: rgba(118,185,0,0.08);
  color: var(--brand-primary);
  font-size: 10px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 1px;
}
```

### Status pill
```css
.pill.pos { background: rgba(46,204,113,0.15); color: #2ecc71; }
.pill.neg { background: rgba(231,76,60,0.15);  color: #e74c3c; }
```

### Grid layouts
```css
.kpi-grid           { display: grid; grid-template-columns: repeat(5, 1fr); gap: 14px; }
.chart-grid-2x2     { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
.chart-grid-3col    { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; }
.annual-grid        { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
```

---

## Number Formatting Helpers

```js
const fmt    = n => n < 0 ? `(${Math.abs(n).toLocaleString()})` : n.toLocaleString();
const fmtB   = n => `$${(n/1000).toFixed(1)}B`;
const fmtPct = n => n.toFixed(1) + '%';
```

---

## Verification Checklist (HTML)

- [ ] All 6 tabs visible and clickable
- [ ] KPI cards show correct latest-FY values
- [ ] All 10+ charts render with correct quarter count (e.g., 16 bars for 16 quarters)
- [ ] FY section rows appear in correct color in tables
- [ ] Filter buttons correctly show/hide quarterly rows
- [ ] Tooltips appear on chart hover with formatted numbers
- [ ] Progress bars in segment table scale correctly to 100%
- [ ] CAGR formula correct: `(endVal/startVal)^(1/N) - 1`
- [ ] No console errors (check browser DevTools)
- [ ] File is self-contained — works after download with no internet (except Chart.js CDN on first load)
