# Financial Performance Dashboard

An executive-ready **Financial Performance Dashboard** built in Power BI to track actual sales, target comparisons, performance variances, sales team metrics, and monthly revenue trends.

---

## 📸 Dashboard Overview

![Financial Performance Dashboard](https://github.com/VinodSingh07/Financial-Performance-Dashboard/blob/main/Finance%20Performance%20Dashboard.png?raw=true)

---

## 🛠️ Project Execution Steps

### Step 1: Data Preparation
* **Connecting Data**: Connected and loaded financial datasets into Power BI.
* **Power Query Transformations**:
  * Cleaned missing values and standardized column data types.
  * Created structured date/calendar attributes for time-intelligence analysis.
  * Formatted salesperson metadata and image URLs for dynamic visual rendering in table views.

---

### Step 2: Data Modelling
* Designed a clean star-schema semantic model in Power BI.
* Defined standard 1-to-Many ($1:*$) relationships between dimension tables (e.g., `Calendar`, `Sales Team`) and core fact tables.
* Configured proper filtering directions across dimensions to enable interactive cross-filtering across visuals.

---

### Step 3: Calculations & DAX Measures
Created custom dynamic measures using **Power Pivot / DAX** for core performance metrics and time-intelligence comparisons:

```dax
// Core Sales Metrics
Total Sales Actual = SUM('Sales'[Actual])
Total Sales Target = SUM('Sales'[Target])

// Variance Calculations
Variance = [Total Sales Actual] - [Total Sales Target]
Variance Pct = DIVIDE([Variance], [Total Sales Target])

// Time Intelligence (YTD)
Total Sales Actual YTD = CALCULATE([Total Sales Actual], DATESYTD('Calendar'[Date]))
Total Sales Target YTD = CALCULATE([Total Sales Target], DATESYTD('Calendar'[Date]))
Variance YTD = CALCULATE([Variance], DATESYTD('Calendar'[Date]))

Variance Pct YTD = 
VAR _ActualYTD = CALCULATE([Total Sales Actual], DATESYTD('Calendar'[Date]))
VAR _TargetYTD = CALCULATE([Total Sales Target], DATESYTD('Calendar'[Date]))
RETURN 
    DIVIDE(_ActualYTD - _TargetYTD, _TargetYTD)

// Target Hit Performance
Months Target Reached = 
CALCULATE(
    COUNTROWS('Calendar'),
    FILTER('Calendar', [Variance] >= 0)
)
```
---

### Step 4: Dashboard Visuals & UI Interactions

#### Using Reference Label feature to add context to KPI cards:
* **Utilized Power BI's New Card Visual** stacked vertically inside a styled left sidebar container.
* **Leveraged Reference Labels** to show YTD sub-metrics (e.g., `YTD: $2M`, `YTD: ($113K)`, `YTD: -4.4%`) directly below main metric values.

#### Win/loss chart & Enhancing variance Labels with Emoji:
* **Incorporated conditional color formatting and status emoji indicators** (🔴 / 🟢) for instant visual identification of target misses and hits.

#### Monthly Trend Graph:
* **Built a dual-series Clustered Column Chart** to display 14-month trend comparisons between Total Sales Actual and Total Sales Target.
* **Enabled data markers** above columns to flag month-by-month target achievement status.

#### Sales Person Performance Table:
* **Designed a detailed performance matrix** including inline Salesperson Pictures.
* **Added in-cell Data Bars** for actual performance and custom Conditional Formatting Data Bars (Green/Red) for Variance %.
* **Embedded dynamic Sparklines** for individual visual trend tracking over time.

#### Adding Smart Narrative with Power BI (AI generated commentary):
* **Integrated Power BI's Smart Narrative AI feature** on the right pane to auto-generate readable text summaries on performance trends and key metric shifts.

#### Final version of the report (with bells and whistles):
* **Implemented custom styled button slicers** (*Delish, Jucies, Tempo, Yummies*) for seamless brand/team cross-filtering alongside clean sidebar container positioning.

---

## 📁 Repository Structure

* `Finance Performance Dashboard.png` — Final dashboard screenshot preview.
* `Finance KPI Dashboard with Power BI.pbix` — Main Power BI report file.
