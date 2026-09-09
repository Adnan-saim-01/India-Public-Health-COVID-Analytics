[Uploading README (1).md…]()
# 🇮🇳 India Public Health & COVID-19 Analytics Dashboard | Power BI

An interactive Power BI dashboard analyzing state-wise COVID-19 trends across 20 major Indian states (March 2020 – December 2022), combining epidemiological data with health-infrastructure indicators to surface actionable, policy-level insights.

> 🔗 **[View Interactive Preview →](#)** *(replace with your published Power BI link or React preview link)*

---

## 📊 Overview

This project goes beyond a standard "case counter" dashboard by layering in:
- **Time-series wave analysis** (1st, 2nd, and 3rd wave comparison)
- **Geo-visualizations** mapping case density and severity by state
- **Health infrastructure correlation** — hospital beds/lakh population vs. case fatality rate
- **Vaccination rollout tracking** alongside case trends

The goal: tell a data story that connects *what happened* (cases/deaths) with *why* (infrastructure, vaccination pace, regional disparity).

---

## 🖼️ Screenshots

| National Overview | State Comparison |
|---|---|
| *(add screenshot)* | *(add screenshot)* |

| Wave Trend Analysis | Health Infrastructure |
|---|---|
| *(add screenshot)* | *(add screenshot)* |

---

## ⚙️ Features

- **4-page interactive report**: National Overview → State Comparison → Time Trend → Health Infrastructure
- **10+ custom DAX measures**, including:
  - Time intelligence: `DATEADD`, `DATESINPERIOD` (3-month moving average, MoM growth %)
  - Ranking: `RANKX` for dynamic state leaderboards
  - Safe-division KPIs: Case Fatality Rate %, Cases per Lakh Population
- **Power Query transformations** for data cleaning, type correction, and custom date/month columns
- **Interactive drill-through, bookmarks, and synced slicers** across pages
- **Scatter analysis** identifying outlier states (low infrastructure + high fatality rate)

---

## 🛠️ Tech Stack

- **Power BI Desktop** — data modeling, DAX, report design
- **Power Query (M)** — data cleaning & transformation
- **DAX** — time intelligence, ranking, KPI calculations

---

## 📁 Dataset

`covid_india_dashboard_data.xlsx` — monthly state-wise records covering:

| Column | Description |
|---|---|
| State, Date | State name & month |
| New/Cumulative Confirmed, Deaths, Recovered | Case counts |
| Tests_Conducted | Estimated monthly testing volume |
| Vaccination_Percent | Cumulative % population vaccinated |
| Hospital_Beds_Per_Lakh | Health infrastructure indicator |

> ⚠️ **Note:** This dataset is a **realistically simulated dataset** built to mirror real-world COVID wave patterns, vaccination rollout timing, and population-based scaling — created for portfolio/practice purposes. It is **not official government data**. For a production version, swap this file with real data from [data.gov.in](https://data.gov.in) or archived [covid19india.org](https://data.covid19india.org) datasets using the same column structure.

---

## 🚀 How to Use

1. Clone this repo
2. Open `covid_india_dashboard.pbix` in Power BI Desktop
3. If prompted, update the data source path to the local `.xlsx` file
4. Explore pages via the tabs; use slicers and chips to filter by year/state

---

## 📈 Key DAX Measures (sample)

```DAX
Case Fatality Rate % = 
DIVIDE([Total Deaths], [Total Confirmed], 0) * 100

3-Month Moving Avg Cases = 
AVERAGEX(
    DATESINPERIOD(DateTable[Date], MAX(DateTable[Date]), -3, MONTH),
    [Total Confirmed]
)

State Rank by Cases = 
RANKX(ALLSELECTED(COVID_Data[State]), [Total Confirmed], , DESC)
```

---

## 🙋 About This Project

Built as a portfolio project to demonstrate Power BI skills relevant to data analyst / BI roles: data modeling, DAX, Power Query, and dashboard storytelling — with a public-health domain twist to stand out from typical sales/finance dashboards.

**Author:** Adnan
**LinkedIn:** [linkedin.com/in/mr-adnan-b04813248](https://linkedin.com/in/mr-adnan-b04813248)
**GitHub:** [github.com/Adnan-saim-01](https://github.com/Adnan-saim-01)
