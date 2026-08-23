# 📱 Mobile Sales Executive Analytics Dashboard
### Built with Microsoft Power BI | Data Analytics Portfolio Project

<br>

[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Excel](https://img.shields.io/badge/Data%20Source-Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)](https://www.microsoft.com/excel)
[![DAX](https://img.shields.io/badge/DAX-Measures-0E7490?style=for-the-badge)](https://learn.microsoft.com/en-us/dax/)

---


## 🎯 Project Overview

This is a **fully interactive, executive-grade Mobile Sales Analytics Dashboard** built in Microsoft Power BI. It analyzes **3,835+ retail transactions** across India's major cities, covering multiple smartphone brands, payment channels, and customer demographics.

The dashboard enables business stakeholders to:
- Monitor **total revenue, units sold, transactions, and average selling price** in real time
- Compare performance **Month-to-Date (MTD)** against historical periods
- Identify **Year-over-Year (YoY)** growth using Same Period Last Year analysis
- Explore **geographic sales distribution** across 15+ Indian cities
- Understand **customer demographics** segmented into age cohorts

---

## ✨ What Makes This Dashboard Unique

This project goes beyond a basic tutorial implementation with the following custom enhancements:

| Feature | Description |
|---------|-------------|
| 🎨 **Executive Theme** | Custom Dark Slate & Cyan palette — `Modern_Corporate_Teal.json` |
| 🖼️ **Monochrome Lucide-style Icons** | Custom-drawn vector icons for each KPI (no clip-art) |
| 👥 **Customer Age Segmentation** | DAX-computed `Age Group` dimension: Gen Z / Millennials / Gen X / Seniors |
| 💰 **Average Order Value (AOV)** | Business KPI computed using safe `DIVIDE` with error handling |
| 🎨 **Per-KPI Accent Colors** | Each card has its own accent bar and tile background color |
| 📊 **Modern Chart Types** | Donut chart instead of pie, clustered bar instead of funnel |
| 🗓️ **Custom Calendar Table** | Dedicated `Custom_Calendar` dimension for reliable Time Intelligence |

---

## 📊 Dashboard Pages

### Page 1 — Executive Overview Dashboard
The main overview page with full interactivity across all KPIs.

**KPI Cards (Top Row):**
| KPI | Measure | Accent Color |
|-----|---------|-------------|
| Total Revenue | `SUMX(Sales_Data, Units Sold × Price Per Unit)` | 🔵 Teal |
| Total Quantity | `SUM(Sales_Data[Units Sold])` | 🔵 Blue |
| Transactions | `COUNTROWS(Sales_Data)` | 🟣 Purple |
| Avg. Selling Price | `AVERAGE(Sales_Data[Price Per Unit])` | 🟠 Amber |

**Visuals:**
- 🗺️ **Bubble Map** — Sales by Indian City (geographical distribution)
- 📈 **Line Chart** — Monthly quantity trend
- 🍩 **Donut Chart** — Payment method breakdown (UPI / Credit Card / Cash / EMI)
- 📊 **Ranked Bar Chart** — Top Mobile Models by Revenue
- 🏔️ **Area Chart** — Sales by Day of the Week pattern
- 📋 **Brand Table** — Matrix of Sales + Transactions per Brand

**Slicers:** Mobile Model | Payment Method | Brand | Month timeline

---

### Page 2 — Month-to-Date (MTD) Report
Tracks daily pacing within the current month using a date drill-down hierarchy.

**Measure:**
```dax
MTD = TOTALMTD([Total_Sales], Custom_Calendar[Date].[Date])
```

**Drill hierarchy:** Year → Quarter → Month → Day

---

### Page 3 — Same Period Last Year (YoY Analysis)
Direct year-over-year performance comparison to identify growth trends.

**Measure:**
```dax
Same Period Last Year =
    CALCULATE([Total_Sales],
        SAMEPERIODLASTYEAR(Custom_Calendar[Date].[Date])
    )
```

---

## 🧮 All DAX Measures

```dax
-- 1. Total Sales Revenue
Total_Sales = SUMX(Sales_Data, Sales_Data[Units Sold] * Sales_Data[Price Per Unit])

-- 2. Total Units Sold
Total_Quantity = SUM(Sales_Data[Units Sold])

-- 3. Total Transactions
Transactions = COUNTROWS(Sales_Data)

-- 4. Average Selling Price
Average_Price = AVERAGE(Sales_Data[Price Per Unit])

-- 5. Average Order Value (custom KPI)
Average Order Value = DIVIDE([Total_Sales], [Transactions], 0)

-- 6. Month-to-Date Sales
MTD = TOTALMTD([Total_Sales], Custom_Calendar[Date].[Date])

-- 7. Same Period Last Year
Same Period Last Year =
    CALCULATE([Total_Sales],
        SAMEPERIODLASTYEAR(Custom_Calendar[Date].[Date])
    )

-- 8. Month-over-Month Growth %
MoM Growth % =
VAR LM = CALCULATE([Total_Sales], DATEADD(Custom_Calendar[Date].[Date], -1, MONTH))
RETURN DIVIDE([Total_Sales] - LM, LM, 0)
```

---

## 🧱 Data Model (Star Schema)

```
                    ┌─────────────────────────┐
                    │     Custom_Calendar      │
                    │  (Date Dimension Table)  │
                    │                          │
                    │  Date, Year, Quarter     │
                    │  Month, Month Name       │
                    │  Day, Day Name           │
                    └────────────┬─────────────┘
                                 │ 1
                                 │
                                 │ *
                    ┌─────────────────────────┐
                    │       Sales_Data         │
                    │    (Fact Table)           │
                    │                          │
                    │  Transaction ID, Date    │
                    │  Brand, Mobile Model     │
                    │  Units Sold, Price/Unit  │
                    │  City, Payment Method    │
                    │  Customer Age, Ratings   │
                    │  Age Group (calculated)  │
                    └─────────────────────────┘
```

**Relationship:** `Custom_Calendar[Date]` → `Sales_Data[Date]` (1-to-Many, Single Direction)

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `Executive_Mobile_Sales_Dashboard_FINAL.pbix` | ✅ Final Power BI dashboard (open this) |
| `Mobile Sales Data.xlsx` | Raw dataset enriched with Age Group & Total Amount columns |
| `Modern_Corporate_Teal.json` | Custom Power BI theme file |

---

## 🚀 How to Open & Use

### Prerequisites
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) (free download)

### Steps
1. **Clone** this repository or download the ZIP
2. **Open** `Executive_Mobile_Sales_Dashboard_FINAL.pbix` in Power BI Desktop
3. If prompted about data source path:
   - Go to **Home → Transform Data → Data source settings**
   - Click **Change Source** → browse to `Mobile Sales Data.xlsx` in this folder
   - Click **Refresh**
4. **Enable Map Visual** *(first time only)*:
   - Go to **File → Options and Settings → Options → Global → Security**
   - Enable **"Use Map and Filled Map visuals"** → OK

### Apply the Executive Theme (Optional)
1. Click the **View** tab in the ribbon
2. Click **Themes → Browse for themes**
3. Select `Modern_Corporate_Teal.json`

---

## 💡 Key Business Insights

1. **Revenue Leaders** — Apple and OnePlus generate the highest total revenue due to premium ASPs, while Xiaomi drives the highest unit volume
2. **Digital-First Payments** — UPI and Credit Card together account for 70%+ of all transactions
3. **Millennial Dominance** — The 25–40 age cohort (Millennials) is the highest-revenue demographic across all brands
4. **Weekend Sales Spike** — Saturday sees the highest transaction volume, 20–30% above midweek average
5. **Geographic Hubs** — Mumbai, Delhi, and Bangalore are the top 3 revenue cities

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Dashboard design, DAX measures, data modeling |
| **DAX** | KPI measures, Time Intelligence (MTD, YoY, MoM) |
| **Power Query (M)** | Data transformation and loading |
| **Microsoft Excel** | Raw data source (3,835 transactions) |
| **Star Schema** | Data architecture pattern |

---

## 👨‍💻 Author

**Nikhil Prakash**
- GitHub: [@Coding-yeager](https://github.com/Coding-yeager)
- Email: nikhilprakash0904@gmail.com

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*Built with ❤️ using Microsoft Power BI*
