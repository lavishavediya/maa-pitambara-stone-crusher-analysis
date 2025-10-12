# Maa Pitambara Stone Crusher – Sales Analysis (Aug 2024 – Aug 2025)

### 🪨 Project Overview
This end-to-end analytics project analyzes the **sales, profit, and transportation cost performance** of *Maa Pitambara Stone Crusher*, located in **Nalkheda, Agar Malwa (M.P.)**.  
It combines **SQL, Python, Excel, and Power BI** to deliver deep insights into business profitability, product performance, and customer patterns.

---

## 🎯 Objectives
- Track monthly **sales, profit, and cost** trends.
- Identify **top customers, products, and villages** by profit.
- Evaluate **payment modes** and transport costs.
- Build a complete workflow using **SQL → Python → Power BI**.

---

## 🧩 Tech Stack
| Tool | Purpose |
|------|----------|
| **SQL (PostgreSQL)** | Data cleaning, staging, and analytical queries |
| **Python (Pandas, Matplotlib, Seaborn)** | Data cleaning and exploratory data analysis |
| **Excel** | Raw data source and formatting |
| **Power BI** | Interactive dashboards & DAX calculations |
| **DAX** | Custom measures for KPIs |

---

## ⚙ Workflow

### 1️⃣ SQL – Data Cleaning & Insights
- Loaded the Excel data into PostgreSQL staging table `sales_data_staging`.
- Casted data types and handled numeric conversions:
  ```sql
  ALTER TABLE sales_data_staging
  ALTER COLUMN total_amount TYPE numeric USING total_amount::numeric;

  Calculated key metrics:

-- Total Sales & Profit
SELECT SUM(total_amount) AS total_sales,
       SUM(total_profit) AS total_profit
FROM sales_data_staging;

-- Top 5 Customers by Sales
SELECT customer_name, SUM(total_amount) AS total_sales
FROM sales_data_staging
GROUP BY customer_name
ORDER BY total_sales DESC
LIMIT 5;

2️⃣ Python – Data Cleaning & EDA

Handled missing values & outliers

Created new columns like month_year

Checked numeric correlations & category trends

import pandas as pd
df = pd.read_excel('data/crusher_data.xlsx')
df['month_year'] = pd.to_datetime(df['date']).dt.strftime('%b %Y')
df.describe()


EDA Visuals:

Profit vs Distance (Scatter Plot)

Monthly Profit Trends

Payment Mode Distribution

Customer-wise Revenue Contribution

3️⃣ Power BI – Dashboard Development

Imported cleaned data

Created DAX measures:

Total Sales = SUM(sales_data_staging[total_amount])
Total Profit = SUM(sales_data_staging[total_profit])
Profit Margin % = DIVIDE([Total Profit],[Total Sales])*100


Added KPIs, line & bar charts, treemap & slicers for:

Product-wise Profit

Payment Mode Analysis

Top Villages & Customers

Monthly Profit Trend

📈 Dashboard Insights
Insight	Observation
Top District	Agar Malwa
Most Profitable Product	6mm Aggregates
Top Customers	Ali Rizvi, Faizan Qureshi, Imran Khan
Dominant Payment Mode	Credit Transactions
Average Transport Cost/Order	₹501.83
Profit Trend	Minor decline in Aug 2025 (possible seasonal dip)

🧮 KPIs (Aug 2024–Aug 2025)
KPI	Value
Total Sales	₹2,61,250
Total Profit	₹52,875
Total Orders	30+
Avg Transport Cost	₹501.83
https://github.com/lavishavediya/maa-pitambara-stone-crusher-analysis/commit/29a70cf68ee2b80d09edc3b1cb767930aa533c8d
