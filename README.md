# FirstAxis Bank — Financial Performance Dashboard
### A Business Analytics Portfolio Project
**Tools Used:** Microsoft Excel, Power Query, PowerPivot, DAX, Power BI  
**Domain:** Commercial Banking, Financial Reporting, Variance Analysis  
**Level:** Intermediate — Built as part of a self-directed data analytics learning journey  

---

## About This Project

This is one of my portfolio projects as I develop my skills in business analytics and financial data modelling. I wanted to go beyond generic datasets and build something that reflects how real organisations, particularly banks, actually track and report financial performance internally.

I created a fictional commercial bank called **FirstAxis Bank**, modelled loosely on the structure of mid-tier African commercial banks like Access Bank and Stanbic IBTC, with four business divisions: Retail Banking, Corporate Banking, Treasury, and Digital Banking.

The goal was simple: take raw monthly financial data, clean and model it properly, and turn it into a dashboard that a CFO or divisional head could actually use to make decisions.

---

## The Business Problem

Imagine you are the Head of Financial Planning & Analysis at FirstAxis Bank. Every month you receive three separate reports:

- A monthly P&L actuals report from each division
- A full-year budget/target file set at the start of the year
- A banking KPIs report covering regulatory and performance ratios

Your job is to consolidate these, identify which divisions are hitting their targets, where costs are running high, and flag any risks before the board meeting.

Currently this is being done manually in Excel, copy-paste, no automation, no relationships between the data. My project automates this entire process and turns it into an interactive dashboard.

---

## Dataset

I created three synthetic CSV datasets that simulate realistic commercial banking figures for FY2024 (January — December):

| File | Description | Rows |
|------|-------------|------|
| `FirstAxis_Actuals.csv` | Monthly revenue, OpEx, NII, provisions, headcount by division | 48 rows |
| `FirstAxis_Budget.csv` | Monthly budget targets for the same metrics | 48 rows |
| `FirstAxis_KPIs.csv` | Monthly banking ratios — NPL, CAR, ROE, ROA, Cost-to-Income | 12 rows |

All figures are in **NGN Millions (NGN M)**.

> **Note:** This data is entirely simulated. It does not represent any real institution. The figures are designed to be realistic,, trending upward across the year with Corporate Banking as the dominant division, to enable meaningful analysis.

---

## What I Built

### Step 1 — Power Query ETL Pipeline
I loaded all three CSV files into Power Query and wrote M code to:
- Promote headers and set correct data types
- Add a `DateKey` column (format: YYYY-MM) to enable date relationships
- Add a `Quarter` column (Q1–Q4) for quarterly filtering
- Calculate `Net_Profit_NGN_M` and `Cost_to_Income_Pct` directly in the query
- Build a `DimCalendar` table with 12 unique monthly rows to serve as the date dimension

### Step 2 — PowerPivot Data Model (Star Schema)
I built a star schema in PowerPivot with:
- **FactFinancials** — the Actuals table as the central fact table
- **DimCalendar** — connected to Actuals, Budget, and KPIs via DateKey
- **FirstAxis_Budget** — connected via both DateKey and Division

This structure allows cross-table filtering, selecting a month in the dashboard filters all three data sources simultaneously.

### Step 3 — DAX Measures
I wrote 10 DAX measures to power the dashboard calculations:

```
Total Revenue = SUM(FirstAxis_Actuals[Revenue_NGN_M])
Total OpEx = SUM(FirstAxis_Actuals[Operating_Expenses_NGN_M])
Total Net Profit = SUM(FirstAxis_Actuals[Net_Profit_NGN_M])
Total NII = SUM(FirstAxis_Actuals[Net_Interest_Income_NGN_M])
Total Provisions = SUM(FirstAxis_Actuals[Loan_Provisions_NGN_M])
Budget Revenue = SUM(FirstAxis_Budget[Revenue_Budget_NGN_M])
Revenue Variance % = DIVIDE([Total Revenue] - [Budget Revenue], [Budget Revenue], 0) * 100
Budget Attainment % = DIVIDE([Total Revenue], [Budget Revenue], 0) * 100
Cost to Income Ratio = DIVIDE([Total OpEx], [Total Revenue], 0) * 100
Net Profit Margin % = DIVIDE([Total Net Profit], [Total Revenue], 0) * 100
```

### Step 4 — Power BI Dashboard (3 Pages)

**Page 1 — Executive Summary**
- 5 KPI cards: Total Revenue, OpEx, NII, Provisions, Headcount
- Line chart: Monthly Revenue Trend Jan–Dec FY2024
- Bar chart: Revenue by Division FY2024

**Page 2 — Division Breakdown**
- Stacked column chart: Revenue vs OpEx by Division
- Donut chart: Revenue Share by Division (%)
- Summary table: Revenue, OpEx, Net Profit, Headcount per division

**Page 3 — Variance Analysis**
- Clustered column chart: Actuals vs Budget by Month
- Line chart: Revenue Trend, Actual vs Budget (Jan–Dec)
- Variance table: Actual vs Budget revenue per division

---

## Key Findings

After building the dashboard, these were the main insights from the data:

1. **Corporate Banking is the dominant revenue driver** — contributing NGN 97.7B (41.8%) of total FY2024 revenue of NGN 233.5B

2. **The bank finished FY2024 below budget** — total actual revenue of NGN 233.5B vs a budget of NGN 237.7B, a shortfall of approximately NGN 4.2B (-1.8%)

3. **All four divisions missed their revenue targets** — Corporate Banking came closest at 98.6% attainment; Digital Banking showed the strongest growth trajectory month-on-month

4. **Digital Banking has the best cost efficiency** — despite being the smallest division by revenue, its cost-to-income ratio improved consistently from H1 to H2

5. **Revenue grew consistently throughout the year** — from NGN 16.9B in January to NGN 22.8B in December, suggesting strong underlying business momentum despite the budget miss

---

## What I Learned

This project honestly took me much longer than expected. I ran into several real issues:

- Power Query M code syntax errors when setting file paths
- The PowerPivot relationship failing because DimCalendar had duplicate DateKeys, fixed by collapsing the table to monthly granularity
- Power BI web vs desktop compatibility issues with the data model
- Many-to-many relationship challenges between Actuals and Budget on the Division column

Working through these problems taught me more than any tutorial would have. I now understand why data modelling decisions made early, like choosing the right granularity for a dimension table, have massive downstream consequences.

---

## Files in This Repository

```
FirstAxis-Bank-Dashboard/
│
├── data/
│   ├── FirstAxis_Actuals.csv
│   ├── FirstAxis_Budget.csv
│   └── FirstAxis_KPIs.csv
│
├── FirstAxis_FinancialModel.pdf
├──FirstAxis_Case Study.docx
├── FirstAxis_FinancialModel.xlsx    ← Power Query + PowerPivot model 
│
└── README.md
```

---

---

## Tools & Skills Demonstrated

| Tool | What I used it for |
|------|--------------------|
| Power Query (M) | ETL — loading, cleaning, transforming 3 CSV sources |
| PowerPivot | Star schema data model, table relationships |
| DAX | 10 calculated measures for KPIs and variance analysis |
| Power BI | 3-page interactive dashboard with charts, cards, tables |
| Excel | Data staging and model hosting |

---

## About Me

I am currently building my skills in data and business analytics, with a focus on financial services and commercial banking. This project is part of a self-directed portfolio I am developing to transition into a Business Analystics or Data Analyst role.

I am actively looking for entry-level opportunities, internships, or graduate programmes in analytics, business intelligence, or financial analysis.

Feel free to connect with me on LinkedIn or reach out via email.

---

*Built with real curiosity and a lot of troubleshooting — April 2026*
