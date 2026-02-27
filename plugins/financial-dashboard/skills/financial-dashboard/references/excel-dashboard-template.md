# Excel Financial Dashboard — Detailed Template Reference

## Workbook Architecture

```
[COMPANY]_Financial_Dashboard_[FY_RANGE].xlsx
├── Sheet 1: Dashboard          (active, green tab)
├── Sheet 2: Quarterly IS       (blue tab)
├── Sheet 3: Annual Summary     (dark green tab)
├── Sheet 4: Segment Analysis   (purple tab)
├── Sheet 5: ChartData          (gray tab — helper, hidden or last)
└── Sheet 6: Sources & Notes    (orange tab)
```

## Color Palette

### Standard Colors
```python
WHITE      = "FFFFFF"
BLACK      = "000000"
LIGHT_GRAY = "F2F2F2"
MED_GRAY   = "D9D9D9"
DARK_GRAY  = "595959"
ALT_ROW    = "EBF3FB"   # alternating row fill

# Headers
HEADER_FILL  = "1F4E79"  # dark blue
SUBHDR_FILL  = "2E75B6"  # medium blue

# Financial modeling convention
INPUT_BLUE   = "0000FF"  # hardcoded input values
FORMULA_BLK  = "000000"  # calculated values / formulas
LINK_GREEN   = "008000"  # cross-sheet references

# Conditional formatting
POSITIVE_BG  = "C6EFCE"  # green background for positive growth
POSITIVE_TXT = "006100"  # green text
NEGATIVE_BG  = "FFC7CE"  # red background for negative
NEGATIVE_TXT = "9C0006"  # red text
ATTENTION_BG = "FFFF00"  # yellow for estimates/attention
```

### Company Brand Colors (customize per company)
```python
# NVIDIA
BRAND_PRIMARY = "76B900"   # NVIDIA green

# Apple
BRAND_PRIMARY = "555555"   # Apple dark gray

# Microsoft
BRAND_PRIMARY = "00A4EF"   # Microsoft blue

# IBM
BRAND_PRIMARY = "006699"   # IBM blue

# Generic tech
BRAND_PRIMARY = "2E75B6"   # medium blue
```

### FY-Specific Colors (for section headers)
Use distinct colors to visually separate fiscal years:
```python
FY_COLORS = {
    "FY2021": "7030A0",  # purple
    "FY2022": "C55A11",  # rust/orange
    "FY2023": "C55A11",  # rust/orange
    "FY2024": "2E75B6",  # medium blue
    "FY2025": "375623",  # dark green
    "FY2026": "76B900",  # brand green (or company color)
}
```

---

## Sheet 1 — Dashboard Layout

### Row 1: Title Banner
```
[Merged A1:Z1]
"[COMPANY] Corporation — Financial Performance Dashboard ([FY_RANGE])"
Font: Arial, 16pt, Bold, White
Fill: Company brand color (e.g., NVDA_GREEN)
Height: 36px
```

### Row 2: Subtitle
```
[Merged A2:Z2]
"Quarterly GAAP financials | All values in USD ($M) | Sources: [IR], SEC | FY ends [month]"
Font: Arial, 9pt, Italic, Dark Gray
Fill: Light Gray (#F2F2F2)
Height: 14px
```

### Rows 4–7: KPI Cards
Place 5 KPI cards across the row, each spanning 5 columns:
- **Title** (row 4): 9pt bold white on brand color
- **Value** (row 5): 18pt bold in brand color (e.g., "$68.1B")
- **Change** (row 6): 12pt bold green (e.g., "+73%")
- **Context** (row 7): 8pt gray (e.g., "YoY")

Standard KPIs:
1. Full-year revenue (most recent FY)
2. Full-year net income (most recent FY)
3. Most recent quarter revenue
4. Gross margin (most recent quarter)
5. Revenue CAGR (full period)

### Rows 9–54: Charts
- Upper-left (A9): Chart 1 — Stacked Revenue bar (20W × 12H)
- Upper-right (N9): Chart 2 — Net Income line (16W × 10H)
- Lower-left (A32): Chart 3 — Margin % dual lines (16W × 10H)
- Lower-right (N32): Chart 4 — Annual Revenue bar (14W × 10H)

---

## Sheet 2 — Quarterly Income Statement

### Column Layout
| Col | Content | Width | Format |
|-----|---------|-------|--------|
| A | Fiscal Quarter Label (e.g., "Q1 FY24") | 12 | Text, Center, Bold Blue |
| B | Period End Date | 16 | Text, Center |
| C | Revenue ($M) | 17 | `#,##0;(#,##0);"-"`, Right |
| D | Cost of Revenue ($M) | 21 | Same |
| E | Gross Profit ($M) | 18 | Same |
| F | Operating Expenses ($M) | 18 | Same |
| G | Operating Income ($M) | 21 | Same |
| H | Net Income ($M) | 17 | Same |
| I | Gross Margin % | 14 | `0.0%;(0.0%);"-"`, Center |
| J | Net Margin % | 12 | Same |
| K | YoY Rev Growth | 14 | Same, conditional color |

### FY Section Headers
Insert one row before each FY's quarters:
```
[Merged A:K] "  FY2024 (Feb 2023 – Jan 2024)"
Font: Arial, 10pt, Bold, White
Fill: FY-specific color
Height: 18px
```

### Alternating Rows
Even-indexed quarters: fill `ALT_ROW` (#EBF3FB)
Odd-indexed quarters: fill `WHITE`

### YoY Growth Conditional Formatting
- > 100%: bright green background (#C6EFCE)
- 30%–100%: light green (#EBFAEB)
- 0%–30%: white/alt row
- < 0%: light red (#FFC7CE)

---

## Sheet 3 — Annual Summary

### Column Layout
| Col | Content | Width |
|-----|---------|-------|
| A | Metric name | 22 |
| B–E | FY values (one per FY) | 14 each |
| F–H | YoY delta for each FY pair | 13 each |
| I | 3-Year CAGR | 11 |
| J | Commentary | 35 |

### Standard Metrics (rows)
1. Revenue ($M)
2. Cost of Revenue ($M)
3. Gross Profit ($M)
4. Gross Margin %
5. Operating Expenses ($M)
6. Operating Income ($M)
7. Net Income ($M)
8. Net Margin %
9. (Optional) Diluted EPS
10. (Optional) R&D Spend ($M)

### CAGR Formula
```
CAGR = (End Value / Start Value)^(1/N) - 1
where N = number of years
```

---

## Sheet 4 — Segment Analysis

### Column Layout
| Col | Content | Width |
|-----|---------|-------|
| A | Quarter | 12 |
| B | Segment 1 ($M) | 20 |
| C | Segment 2 ($M) | 14 |
| D | Segment 3 ($M) | 16 |
| E | Segment 4 ($M) | 16 |
| F | Other ($M) | 14 |
| G | Total ($M) | 18 |
| H | Primary Segment % | 18 |

Conditional color on col H: >85% bright green, >70% light green.

Common segment structures:
- **NVIDIA**: Data Center, Gaming, Professional Visualization, Automotive, OEM
- **Apple**: iPhone, Mac, iPad, Wearables/Home/Accessories, Services
- **Microsoft**: Productivity/Business Processes, Intelligent Cloud, Personal Computing
- **IBM**: Software, Consulting, Infrastructure
- **Amazon**: AWS, North America Retail, International Retail

---

## Chart Configuration Reference

### Chart 1: Stacked Revenue Bar
```python
chart = BarChart()
chart.type = "col"
chart.grouping = "stacked"
chart.title = "Quarterly Revenue: Cost of Revenue vs. Gross Profit"
chart.y_axis.numFmt = '#,##0'
# CoR series: muted color (e.g., rust "#C55A11")
# Gross Profit series: brand primary color
```

### Chart 2: Net Income Line
```python
chart = LineChart()
chart.title = "Quarterly Net Income Trend"
chart.y_axis.numFmt = '#,##0'
# Single smooth line, medium blue (#2E75B6)
# line.width = 25000 (2.5pt)
```

### Chart 3: Dual Margin Lines
```python
chart = LineChart()
chart.title = "Gross Margin % vs Net Margin %"
chart.y_axis.numFmt = '0%'
# Gross Margin: brand color, 2.5pt
# Net Margin: medium blue, 2pt
```

### Chart 4: Annual Revenue Bar
```python
chart = BarChart()
chart.type = "col"
chart.title = "Annual Revenue — [FY_RANGE]"
chart.y_axis.numFmt = '#,##0'
# Single color bars: brand primary
```

### ChartData Sheet Structure
Use a dedicated helper sheet to store chart data arrays cleanly:
```
Row 1: Quarter labels (headers)
Row 2: Revenue
Row 3: Cost of Revenue
Row 4: Gross Profit
Row 5: Net Income
Row 6: Gross Margin %
Row 7: Net Margin %
Row 8: Operating Income
Row 10: FY labels
Row 11: Annual Revenue
Row 12: Annual Net Income
Row 13: Annual Gross Profit
```
Charts reference this sheet so the Dashboard stays clean.

---

## Sources Sheet Structure

### Source Table (one row per quarter/period)
| Col A | Col B | Col C |
|-------|-------|-------|
| Period | Source Name | URL |

### Notes Table
| Col A | Col B | Col C |
|-------|-------|-------|
| Topic | Description | (blank or URL) |

Standard notes to always include:
1. Fiscal year definition and calendar mapping
2. Any one-time charges or non-GAAP adjustments affecting a quarter
3. Estimated vs. reported figures
4. Currency and unit conventions
5. Color coding legend

---

## openpyxl Implementation Patterns

### Creating headers
```python
cell = ws.cell(row=1, column=1, value="Title")
cell.font = Font(name="Arial", bold=True, size=14, color="FFFFFF")
cell.fill = PatternFill("solid", fgColor="1F4E79")
cell.alignment = Alignment(horizontal="center", vertical="center")
```

### Currency format
```python
cell.number_format = '#,##0;(#,##0);"-"'
```

### Percentage format
```python
cell.number_format = '0.0%;(0.0%);"-"'
```

### Conditional coloring
```python
def growth_color(value):
    if value > 1.0: return "C6EFCE"   # >100% bright green
    elif value > 0: return "EBFAEB"   # positive light green
    else: return "FFC7CE"              # negative light red
```

### Freeze panes (lock header rows)
```python
ws.freeze_panes = "C4"  # freeze rows 1-3 and columns A-B
```

### Hide gridlines
```python
ws.sheet_view.showGridLines = False
```

### Set column width
```python
ws.column_dimensions["A"].width = 20
```

### Set row height
```python
ws.row_dimensions[1].height = 36
```

### Merge cells
```python
ws.merge_cells("A1:Z1")
```

### Tab color
```python
ws.sheet_properties.tabColor = "76B900"
```

### Chart series color
```python
ser = chart_mod.Series(data_ref, title="Series Name")
ser.graphicalProperties.solidFill = "76B900"  # for bar charts
ser.graphicalProperties.line.solidFill = "76B900"  # for line charts
ser.graphicalProperties.line.width = 25000  # 2.5pt
ser.smooth = True  # for line charts
```
