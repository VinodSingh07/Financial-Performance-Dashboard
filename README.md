# Financial Performance Dashboard

An executive-ready **Financial Performance Dashboard** built in Power BI to track actual sales, target comparisons, performance variances, sales team metrics, and monthly revenue trends.

---

## 📸 Dashboard Overview

![Financial Performance Dashboard](Finance%20Performance%20Dashboard.jpg)

---

## 🛠️ Project Execution Steps

### 1. Data Preparation
* **Connecting Data**: Connected and loaded financial datasets into Power BI.
* **Power Query Transformations**:
  * Cleaned missing values and standardized column data types.
  * Created structured date/calendar attributes for time-intelligence analysis.
  * Formatted sales person metadata and image URLs for dynamic visual rendering in table views.

---

### 2. Data Modelling
* Designed a clean star-schema semantic model in Power BI.
* Defined standard 1-to-Many ($1:*$) relationships between dimension tables (e.g., `Calendar`, `Sales Team`) and core fact tables.
* Configured proper filtering directions across dimensions to enable interactive cross-filtering across visuals.

---

### 3. Calculations & DAX Measures
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
