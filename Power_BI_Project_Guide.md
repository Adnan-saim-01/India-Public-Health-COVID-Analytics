# India COVID & Public Health Analytics Dashboard — Power BI Build Guide

**Dataset:** `covid_india_dashboard_data.xlsx` (20 states × 34 months, Mar 2020 – Dec 2022)
**Level:** Intermediate | **Time to build:** ~2-3 hours

---

## Step 1 — Import Data

1. Power BI Desktop → **Home → Get Data → Excel Workbook**
2. Select `covid_india_dashboard_data.xlsx` → check only the **"Data"** sheet → Load
3. In **Power Query Editor** (Transform Data), do this to show data-cleaning skill on resume:
   - Change `Date` column type to **Date** (not Date/Time)
   - Ensure all numeric columns are typed **Whole Number** / **Decimal Number**
   - Add a custom column: `Year = Date.Year([Date])`
   - Add a custom column: `Month Name = Date.MonthName([Date])`
   - Rename the query to `COVID_Data`
   - Click **Close & Apply**

## Step 2 — Create a Date Table (shows you know time intelligence properly)

Modeling tab → **New Table**:
```
DateTable = CALENDAR(MIN(COVID_Data[Date]), MAX(COVID_Data[Date]))
```
Then add columns:
```
Year = YEAR(DateTable[Date])
MonthNo = MONTH(DateTable[Date])
MonthName = FORMAT(DateTable[Date], "MMM YYYY")
```
Mark this table as a **Date Table** (Table tools → Mark as Date Table).
Go to Model view → drag a relationship from `DateTable[Date]` → `COVID_Data[Date]`.

## Step 3 — Add a State Geo Table (needed for the map)

You'll need a small mapping table with state names + India state codes/lat-long for the map visual to plot correctly. Add a new table (Enter Data) with columns: `State`, `Latitude`, `Longitude` (search "India state capitals lat long" for reference values), OR simpler: use Power BI's built-in **Shape Map** visual with a state-name-matched TopoJSON (search GitHub "india states topojson" — free, one-time download, import via Format → Map settings).

*Simplest for intermediate level:* use the standard **Map** visual (bubble map) with State name as location — Power BI's Bing-based geocoding auto-resolves major Indian state names reasonably well.

---

## Step 4 — DAX Measures (create a separate "Measures" table for clean organization)

```DAX
Total Confirmed = SUM(COVID_Data[New_Confirmed])

Total Deaths = SUM(COVID_Data[New_Deaths])

Total Recovered = SUM(COVID_Data[New_Recovered])

Case Fatality Rate % = 
DIVIDE([Total Deaths], [Total Confirmed], 0) * 100

Cases Per Lakh Population = 
DIVIDE([Total Confirmed], SUM(COVID_Data[Population]), 0) * 100000

MoM Growth % = 
VAR CurrentMonth = [Total Confirmed]
VAR PrevMonth = CALCULATE([Total Confirmed], DATEADD(DateTable[Date], -1, MONTH))
RETURN DIVIDE(CurrentMonth - PrevMonth, PrevMonth, 0) * 100

3-Month Moving Avg Cases = 
AVERAGEX(
    DATESINPERIOD(DateTable[Date], MAX(DateTable[Date]), -3, MONTH),
    [Total Confirmed]
)

Cumulative Confirmed (Running Total) = 
CALCULATE(
    [Total Confirmed],
    FILTER(ALLSELECTED(DateTable), DateTable[Date] <= MAX(DateTable[Date]))
)

State Rank by Cases = 
RANKX(ALLSELECTED(COVID_Data[State]), [Total Confirmed], , DESC)

Avg Vaccination % = AVERAGE(COVID_Data[Vaccination_Percent])

Avg Hospital Beds per Lakh = AVERAGE(COVID_Data[Hospital_Beds_Per_Lakh])

Tests Per Case = DIVIDE(SUM(COVID_Data[Tests_Conducted]), [Total Confirmed], 0)
```

These 10 measures alone show CALCULATE, FILTER, DATEADD, RANKX, DIVIDE with safe zero-handling, and moving averages — solid intermediate DAX range for a fresher resume.

---

## Step 5 — Page Layout (4 pages)

### Page 1: National Overview
- Top KPI cards: Total Confirmed, Total Deaths, Case Fatality Rate %, Avg Vaccination %
- Line chart: Cumulative Confirmed over time (by MonthName)
- Map visual: bubble size = Total Confirmed, colour = Case Fatality Rate %
- Slicers: Year, State (sync across pages via Format → Edit Interactions)

### Page 2: State Comparison
- Bar chart: Top 10 states by Total Confirmed
- Table with conditional formatting (data bars) — State, Total Confirmed, CFR%, Rank
- Scatter chart: Cases Per Lakh Population (X) vs Case Fatality Rate % (Y), bubble size = Population → identifies outlier states (great talking point in interview)

### Page 3: Time Trend / Wave Analysis
- Line chart: New_Confirmed by month, split by Year (2020/2021/2022 as separate lines) — shows 1st/2nd/3rd wave comparison
- 3-Month Moving Avg line overlaid on raw monthly line
- MoM Growth % as a column chart

### Page 4: Health Infrastructure (differentiator page)
- Bar chart: Hospital Beds per Lakh by state
- Combo chart: Vaccination % vs Case Fatality Rate % by state (does more beds/vaccination correlate with lower CFR?)
- A text box with 2-3 line "Key Insight" callout, e.g. "States with <60 beds/lakh show ~1.4x higher fatality rate" (write based on what your actual data shows)

---

## Step 6 — Visual Polish (what separates "fresher project" from "intermediate/attractive")

- Pick ONE consistent theme: View → Themes → try "City Park" or import a custom JSON theme (search "Power BI free themes")
- Consistent color coding: red/orange = deaths/severity, green = recovered/vaccinated, blue = confirmed/neutral — keep this same across ALL pages
- Use **Tooltips**: create a small tooltip page showing extra state details on hover
- Add a **Bookmark** for "2nd Wave View" vs "Full Timeline View" and buttons to switch — shows interactivity skill
- Turn off default visual borders, add subtle shadows for a cleaner look (Format pane → Effects)
- Title each page clearly + add a small subtitle explaining what the page answers

---

## Step 7 — Publish & Showcase

1. File → Publish → Publish to Power BI service (free account) — get a shareable link
2. Take 3-4 clean screenshots of different pages
3. Record a 60-90 second Loom/screen-recording walkthrough explaining one insight
4. Upload `.pbix` to GitHub with a README (data source disclaimer, screenshots, what each DAX measure does)

---

## Resume Bullet Points (ATS-friendly, ready to paste)

> **India Public Health & COVID Analytics Dashboard (Power BI)**
> - Built an interactive 4-page Power BI dashboard analyzing state-wise COVID trends across 20 Indian states and 34 months, using Power Query for data cleaning and modeling
> - Designed 10+ DAX measures including time-intelligence (moving averages, MoM growth, running totals) and ranking functions (RANKX) to surface state-level insights
> - Created geo-visualizations (map, scatter) correlating health infrastructure (hospital beds/lakh) and vaccination rates with case fatality rate, identifying actionable outlier states
> - Applied UX principles — bookmarks, drill-through, consistent theming, and tooltips — to improve dashboard usability and storytelling

---

## Important Honesty Note

The dataset provided (`covid_india_dashboard_data.xlsx`) is **synthetically generated** to realistically mimic real-world patterns (waves, vaccination rollout timing, population-based scaling) — it is NOT official government data. This is fine for portfolio/practice purposes, but:
- **Don't claim it's real government data** in interviews — instead say "built using a realistic simulated dataset modeled on data.gov.in/WHO patterns" if asked directly.
- If you want the resume-safest version, replace this with actual data.gov.in or covid19india.org CSVs (same column structure) before final submission — I can help you restructure real data into this same model if you download it.
