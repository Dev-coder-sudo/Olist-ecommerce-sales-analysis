# Olist E-Commerce Sales Analysis

End-to-end analytics project · Brazilian marketplace · 2016–2018

Python Pandas SQL · DuckDB Matplotlib · Seaborn ✓ 96,469 orders analysed ✓ 10 SQL queries ✓ RFM segmentation

## Overview

A full-cycle data analytics project on the publicly available [Olist Brazilian E-Commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle). The project covers data cleaning, exploratory data analysis across six business dimensions, customer segmentation with RFM, and ten SQL queries demonstrating advanced window functions and CTEs.

The goal is to surface actionable insights a growth or operations team could act on — not just descriptive statistics — with every finding backed by reproducible code.

## Key Findings

Revenue Peak

R$1.15M

Nov 2017 — Black Friday (+53.5% MoM)

Growth Rate

+728%

New customers Jan 2017 → Jul 2018

Repeat Buyers

3%

But 1.9× higher lifetime value (R$307 vs R$161)

Review Score

4.16★

Delivery time is #1 driver (r = −0.335)

Delivery Gap

18 days

São Paulo 7d vs Amazonas 25d

At-Risk Revenue

34.7%

RFM "At Risk" segment outweighs Champions (30.3%)

**Standout finding:** The At-Risk RFM segment — historically high-value customers who have not returned — drives more total revenue (34.7%) than Champions (30.3%). A re-engagement campaign targeting this cohort has higher ROI potential than acquiring net-new customers.

## Repository Structure

ecommerce-sales-analysis/ ├── notebooks/ │ ├── 01_Olist_Sales_Cleaned.ipynb ← cleaning pipeline │ ├── 02_Olist_Sales_EDA.ipynb ← six-section EDA │ └── 03_Olist_SQL_Analysis.ipynb ← 10 SQL queries via DuckDB ├── clean/ │ ├── master_orders.csv ← 96,469 rows × 24 cols (delivered orders) │ ├── master_products.csv ← 112,650 rows × 13 cols (item-level grain) │ ├── reviews_text_clean.csv │ └── rfm_segments.csv ← RM-scored customer segments └── README.md

## Notebooks

### 01 · Data Cleaning

Joins seven Olist CSV files into two analysis-ready dataframes. Key decisions: filter to `delivered` orders only (96,469); use `customer_unique_id` (not `customer_id`) for person identity; resolve 764 duplicate review IDs across orders and 543 duplicate order submissions; fill null product categories with `'unknown'` rather than dropping (products still have real sales data).

### 02 · Exploratory Data Analysis

| Section | Topic | Highlight finding |
| --- | --- | --- |
| 1 | Revenue & Time | Volume-driven growth; AOV stable at R$146–170 throughout |
| 2 | Products | `health_beauty` #1 revenue; `watches_gifts` premium outlier (94% AOV premium) |
| 3 | Customers | 97% one-time buyers; Black Friday cohort underperformed on retention (0.57% vs 0.72%) |
| 4 | Geography | SP = 37% revenue; remote states (PB, AC) spend more per order despite longer delivery |
| 5 | RFM Segmentation | At-Risk segment holds 34.7% of revenue — highest of any segment |
| 6 | Reviews | 10.7-day delivery gap between 1★ and 5★ orders; Champions rate lowest (elevated expectations) |

### 03 · SQL Analysis (DuckDB)

Ten queries written in DuckDB, executed in-notebook against the cleaned CSVs. No external database required.

| # | Query | SQL concepts used |
| --- | --- | --- |
| Q1 | Monthly revenue + MoM growth | CTE, `LAG()` |
| Q2 | Rolling 3-month average | `AVG() OVER (ROWS BETWEEN 2 PRECEDING …)` |
| Q3 | Quarterly revenue + QoQ growth | `PARTITION BY`, `CASE WHEN`, % of year |
| Q4 | Top 10 customers by LTV | `RANK()` on two dimensions, `DATEDIFF`, `SUM() OVER ()` |
| Q5 | Repeat vs one-time revenue split | 3 chained CTEs, window-function percentages |
| Q6 | Days between 1st and 2nd purchase | `ROW_NUMBER() OVER PARTITION BY`, distribution buckets |
| Q7 | Revenue rank within category | `RANK()` + `DENSE_RANK()`, nested windows |
| Q8 | Late delivery rate by category | `COUNT(DISTINCT CASE WHEN …)` to handle join fan-out |
| Q9 | Delivery days by state vs national avg | `CROSS JOIN` benchmark CTE, `STDDEV()`, 3× `RANK()` |
| Q10 | New customer acquisition per month | `QUALIFY ROW_NUMBER()`, cumulative `SUM() OVER (ROWS UNBOUNDED)` |

## Tech Stack

Python 3 Pandas NumPy Matplotlib Seaborn DuckDB Google Colab Jupyter

## How to Run

All notebooks run on **Google Colab** (no local setup required).

### 1. Get the data

Download the [Olist dataset from Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) and upload to your Colab session or mount Google Drive.

### 2. Run in order

```
01_Olist_Sales_Cleaned.ipynb   # generates clean/master_orders.csv + master_products.csv
02_Olist_Sales_EDA.ipynb        # reads from clean/; generates rfm_segments.csv
03_Olist_SQL_Analysis.ipynb     # reads from clean/; requires duckdb (pip install duckdb)
```

### 3. DuckDB install

```
!pip install duckdb
```

## Dataset

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — available on Kaggle under a [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) licence. The dataset covers \~100k orders placed on the Olist marketplace between 2016 and 2018, including order status, price, payment, freight, customer location, product attributes, and customer reviews.

**Note on scope:** EDA uses `master_orders_stable` (Jan 2017–Jul 2018), which excludes the soft-launch period and the incomplete final month. SQL queries run on the full `master_orders.csv` (89,851 rows). The 97/3 repeat-buyer split is consistent across both scopes.

## Author

Built by **Dev Gupta** as a portfolio project for transitioning into a Data Analyst role. \
[LinkedIn](https://linkedin.com/in/) · [GitHub](https://github.com/)