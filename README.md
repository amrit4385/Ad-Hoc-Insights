# Consumer Goods: Ad-Hoc Analysis

This repository serves as documentation for the **Consumer Goods: Ad-Hoc Analysis - MySQL Project**. It was created as part of **Resume Project Challenge 4: Provide Insights to Management in the Consumer Goods Domain** by **Codebasics**.

The entire project has been implemented using **MySQL Workbench 8.0 CE**.

**Note:** The raw data files have not been uploaded to this repository in compliance with Codebasics' Data & Content Distribution Policy.

## Contents
- [Introduction to AtliQ Hardware](#introduction-to-atliq-hardware)
- [Project Objective](#project-objective)
- [Tools & Methodologies](#tools--methodologies)
- [About the Dataset](#about-the-dataset)
- [Data Dictionary](#data-dictionary)
- [Data Model - ERD](#data-model---erd)
- [Project Implementation](#project-implementation)
- [Ad-Hoc Analysis Insights](#ad-hoc-analysis-insights)
- [Conclusion & Recommendations](#conclusion--recommendations)

---
## Introduction to AtliQ Hardware

**Domain:** Consumer Goods | **Functions:** Finance & Supply Chain

AtliQ Hardware is a company that sells computer hardware and peripherals like **PCs, mice, printers, etc.** to clients globally. Their primary **B2B** business model involves selling products to retailers like **Croma, Best Buy, Staples, and Flipkart**, who then sell them to consumers.

**Sales Channels:**
- **Retailer (Brick & Mortar & E-commerce)**: Traditional and online retailers.
- **Direct (B2C)**: AtliQ E-store & AtliQ Exclusive.
- **Distributor**: Serves restricted trade regions (e.g., Neptune).

---
## Project Objective

Business growth analysis is essential for companies to remain competitive. The goal of this project is to write effective **SQL queries** to answer **10 ad-hoc business requests** from the Data Analytics Director. These insights help AtliQ Hardware understand sales trends, customer segmentation, and product performance.

---
## Tools & Methodologies

### **Tools Used**
- **MySQL Workbench** – Data Cleaning, Manipulation, and Analysis.
- **Datawrapper** – Creating Data Visualizations.
- **Miro** – Entity Relationship Diagram (ERD) Design.
- **PowerPoint** – Project Presentation.
- **GitHub** – Documentation.

### **Key Methodologies Implemented**
- **Data Cleaning** – CRUD Operations.
- **Data Manipulation** – Subqueries, CTEs, Views, Window Functions, UDFs, Stored Procedures, Triggers.
- **Database Modeling & Normalization** – ERD.
- **Exploratory Data Analysis (EDA)** – Filtering, Aggregation, Grouping, Joins.
- **Data Visualization & Reporting**.

---
## About the Dataset

### **Data Sources:** Finance & Supply Chain

The dataset contains **9 tables**:
- `dim_customer` – 209 records | 7 columns.
- `dim_product` – 397 records | 6 columns.
- `fact_forecast_monthly` – 1,880,064 records | 5 columns.
- `dim_freight_cost` – 135 records | 4 columns.
- `dim_gross_price` – 1,182 records | 3 columns.
- `dim_manufacturing_cost` – 1,182 records | 3 columns.
- `dim_post_invoice_deductions` – 2,057,704 records | 5 columns.
- `dim_pre_invoice_deductions` – 1,045 records | 3 columns.
- `fact_sales_monthly` – 1,436,905 records | 5 columns.

### **Data Integrity (ROCCC Evaluation)**
- **Reliability:** MEDIUM – Data provided by Codebasics.
- **Originality:** HIGH – First-party provider.
- **Comprehensiveness:** HIGH – Covers multiple dimensions of **customers, products, finance, and supply chain**.
- **Currentness:** LOW – Data is up to **FY 2022**, making trends general rather than recent.
- **Citation:** LOW – No official reference available.

### **Data Dictionary & ERD**
- **[Data Dictionary](https://github.com/amrit4385/Ad-Hoc-Insights/blob/main/Data%20Model/Data%20Dictionary.md)
- **[Entity Relationship Diagram (ERD)](#)** (Add link here)

---
## Project Implementation

### **Phase-wise Execution**
1. **Data Import & Cleaning**
2. **Database Design & ERD Creation**
3. **Calculating Fiscal Year & Quarter**
4. **Finance & Sales Analytics**
5. **Query Optimization & Indexing**
6. **Creating Database Views**
7. **Generating Business Reports**
8. **Ad-Hoc Analysis Insights**

---
## Ad-Hoc Analysis Insights

### **Sample Business Queries Answered:**

1. **Markets where "Atliq Exclusive" operates in APAC**
```sql
SELECT DISTINCT(market) 
FROM dim_customer 
WHERE customer = "Atliq Exclusive" AND region = "APAC";
```

2. **Percentage increase in unique products (2021 vs. 2020)**
```sql
WITH uniqprod_2020 AS (
  SELECT COUNT(DISTINCT product_code) AS prodcnt_2020 
  FROM fact_sales_monthly WHERE fiscal_year = 2020
), 
uniqprod_2021 AS (
  SELECT COUNT(DISTINCT product_code) AS prodcnt_2021 
  FROM fact_sales_monthly WHERE fiscal_year = 2021
)
SELECT prodcnt_2020, prodcnt_2021, 
       ROUND(((prodcnt_2021 - prodcnt_2020)/prodcnt_2020)*100, 2) AS prodcnt_inc_pct 
FROM uniqprod_2020, uniqprod_2021;
```

3. **Top 3 products in each division with highest sold quantity (2021)**
```sql
WITH prod_sold_qty AS (
  SELECT dp.product_code, dp.product, dp.division,
         SUM(fsm.sold_quantity) AS total_sold_qty
  FROM fact_sales_monthly fsm
  JOIN dim_product dp USING (product_code)
  WHERE fsm.fiscal_year = 2021
  GROUP BY dp.product_code, dp.product, dp.division
), 
div_sold_qty_ranking AS (
  SELECT division, product_code, product, total_sold_qty,
         DENSE_RANK() OVER(PARTITION BY division ORDER BY total_sold_qty DESC) AS sold_qty_rank
  FROM prod_sold_qty
)
SELECT * FROM div_sold_qty_ranking WHERE sold_qty_rank <= 3;
```

(Include more queries as needed)

---
## Conclusion & Recommendations

### **Key Takeaways:**
- **Expand product offerings** in low-count segments (**Desktops, Networking, Storage**).
- **Negotiate better pre-invoice discounts** for high-discount customers (Flipkart, Viveks, Ezone).
- **Strengthen sales channel relationships** and implement strategic marketing campaigns.
- **Invest in R&D** to match evolving consumer demands.

This analysis provides valuable insights to help AtliQ Hardware make data-driven decisions and enhance business performance.

---
## 📌 Next Steps
- Optimize queries for better performance.
- Integrate more visualization tools.
- Automate reporting using **Python & Power BI**.

---
### **🚀 Author: Amritpal Singh**
