# 📱 Mobile Sales Executive Analytics Dashboard
## Complete Project Walkthrough & Interview Guide

---

### 1. 📌 Project Summary (Elevator Pitch)
> *"This project is an end-to-end Retail Mobile Sales Analytics Dashboard built in Power BI to analyze over 3,800 retail transactions across multiple smartphone brands (Apple, Samsung, OnePlus, Vivo, Xiaomi), 15+ metropolitan cities, and key payment channels.*
>
> *The objective was to help business leadership monitor sales revenue, track volume growth, understand customer demographics, and identify high-performing brands and channels across time periods."*

---

### 2. 🗄️ Data Architecture & Modeling (Star Schema)

#### **Tables in Model:**
1. **`Sales_Data` (Fact Table)**: Contains transaction-level records:
   - `Transaction ID`, `Date`, `Brand`, `Mobile Model`, `Units Sold`, `Price Per Unit`, `Customer Name`, `Customer Age`, `City`, `Payment Method`, `Customer Ratings`.
2. **`Custom_Calendar` (Dimension Table)**: Dedicated Date dimension table:
   - `Date`, `Year`, `Quarter`, `Month`, `Month Name`, `Day`, `Day Name`.

#### **Relationships:**
* **`Custom_Calendar[Date]` $\rightarrow$ `Sales_Data[Date]`** (1-to-Many Relationship, Single Direction Filter).

---

### 3. 📐 All DAX Formulas & Logic

#### **A. Core Business Measures**
```dax
-- 1. Total Sales Revenue (Row-by-row iteration of Units * Price)
Total_Sales = SUMX(Sales_Data, Sales_Data[Units Sold] * Sales_Data[Price Per Unit])

-- 2. Total Volume Sold
Total_Quantity = SUM(Sales_Data[Units Sold])

-- 3. Total Transaction Count
Transactions = COUNTROWS(Sales_Data)

-- 4. Average Unit Price
Average_Price = AVERAGE(Sales_Data[Price Per Unit])

-- 5. Average Order Value (AOV) [Upgraded KPI]
Average Order Value = DIVIDE([Total_Sales], [Transactions], 0)
```

#### **B. Time Intelligence Measures**
```dax
-- 6. Month-to-Date (MTD) Sales
MTD = TOTALMTD([Total_Sales], Custom_Calendar[Date].[Date])

-- 7. Same Period Last Year (YoY Comparison)
Same Period Last Year = CALCULATE([Total_Sales], SAMEPERIODLASTYEAR(Custom_Calendar[Date].[Date]))
```

#### **C. Customer Demographics (Calculated Column in `Sales_Data`)**
```dax
Age Group = 
SWITCH(
    TRUE(),
    Sales_Data[Customer Age] < 25, "Under 25 (Gen Z)",
    Sales_Data[Customer Age] <= 40, "25-40 (Millennials)",
    Sales_Data[Customer Age] <= 55, "41-55 (Gen X)",
    "55+ (Seniors)"
)
```

---

### 4. 🎨 Visual Layout & Dashboard Pages

#### **Page 1: Executive Overview Dashboard**
* **Top KPI Bar**: Total Sales, Total Quantity, Transactions, Average Order Value (AOV).
* **Customer Demographics (New)**: Clustered Column Chart showing `Total_Sales` by `Age Group` and `Brand`.
* **Payment Channel Distribution**: Donut Chart showing % share of transactions across UPI, Credit Card, Cash, etc.
* **Geographic Distribution**: Bubble Map of Indian Cities sized by `Total_Sales`.
* **Product Performance**: Horizontal Bar chart ranking top selling `Mobile Model`s.
* **Customer Satisfaction**: Horizontal Bar chart of `Customer Ratings` with conditional color formatting (Green for 4-5, Amber for 3, Red for 1-2).

#### **Page 2: Month-to-Date (MTD) Tracking**
* Time hierarchy drill-down (`Year` $\rightarrow$ `Quarter` $\rightarrow$ `Month` $\rightarrow$ `Day`) to monitor intra-month pacing.

#### **Page 3: YoY & Same Period Last Year (SPLY) Analysis**
* Comparative matrix and column charts showing current year vs. prior year performance.

---

### 5. 🎯 Key Business Insights to Highlight in Interviews

1. **Brand Dominance**: Apple and Samsung account for the majority of total revenue due to higher Average Selling Price (ASP), while Xiaomi and Vivo drive high unit volumes.
2. **Digital Payments Surge**: UPI and Credit Cards account for over **70%** of total transaction volume.
3. **Demographic Shift**: The **25–40 (Millennial)** cohort generates over **50%** of premium smartphone purchases.
4. **Weekend vs. Weekday Patterns**: Friday and Saturday show a significant spike in transaction volume compared to mid-week.

---

### 6. 💼 Top 10 Technical Interview Questions & Answers

#### **Q1: Why did you use `SUMX` instead of `SUM` for `Total_Sales`?**
> **Answer:** *"The raw data has separate columns for `Units Sold` and `Price Per Unit`. `SUM` can only aggregate a single existing column. `SUMX` is an iterator function that calculates `Units Sold * Price Per Unit` at the row level and then sums up the result across the table, avoiding the need for an unnecessary calculated column in the physical table."*

#### **Q2: Why create a dedicated `Custom_Calendar` table instead of relying on Power BI's automatic date hierarchy?**
> **Answer:** *"Using a dedicated Date table is a standard best practice. It optimizes tabular engine memory by disabling auto-date tables for every date column, ensures contiguous date ranges required for Time Intelligence functions (`TOTALMTD`, `SAMEPERIODLASTYEAR`), and allows custom fiscal years or week hierarchies."*

#### **Q3: Why did you use `DIVIDE([Total_Sales], [Transactions], 0)` instead of `[Total_Sales] / [Transactions]`?**
> **Answer:** *"The `DIVIDE` function natively handles divide-by-zero errors without crashing or displaying `NaN`/`Infinity` in visuals. Passing `0` as the third parameter provides a clean fallback value."*

#### **Q4: What data modeling methodology did you follow?**
> **Answer:** *"I followed a Star Schema architecture, separating transactional fact data (`Sales_Data`) from dimensions (`Custom_Calendar`). This ensures high compression in the VertiPaq engine and simplified filter propagation."*

#### **Q5: How did you ensure good user experience (UX) and report performance?**
> **Answer:** *"I kept visual interactions snappy by reducing visual clutter, applying an executive theme with high contrast and consistent typography, formatting all slicers to single-select or dropdowns to minimize query overhead, and using Donut/Bar charts instead of cluttered 3D or funnel charts."*
