
---

# 📈 Customer Lifetime Value & Retention Dashboard (MySQL + Tableau)

A mid-sized **online retail company** wants to understand which customers drive the most value and how retention changes over time.
The marketing team has noticed that **repeat purchases drop significantly after the first few months** and wants data-backed insights to guide loyalty campaigns and re-engagement offers.

I've been tasked with:

* Calculating **Customer Lifetime Value (CLV)** for each customer
* Identifying **top-spending customers**
* Mapping **cohort-based retention trends** over 12 months
* Providing **actionable recommendations** to boost loyalty and reduce churn

This project uses **MySQL** for data processing and **Tableau** for visualization to deliver a clear, interactive dashboard that supports strategic decision-making.

---

## 📊 Project Overview

Customer retention and high-value client management are critical for sustainable growth.
This project uses **SQL** to extract and process transactional data, then visualizes it in **Tableau** to:

* Pinpoint top-spending customers
* Track monthly retention across customer cohorts
* Identify drop-off points and loyalty patterns
* Support targeted marketing and re-engagement strategies

---

## 🎯 Business Goals

* Calculate lifetime value for each customer
* Identify top 10 customers by revenue
* Group customers into cohorts based on first purchase month
* Track monthly retention and churn across cohorts
* Provide actionable recommendations for improving retention

---

## 🗂️ Dataset Summary

The dataset simulates one year of retail transactions for 100 customers.

| Column        | Description                   |
| ------------- | ----------------------------- |
| `order_id`    | Unique order identifier       |
| `customer_id` | Unique customer identifier    |
| `order_date`  | Date of purchase              |
| `quantity`    | Units purchased               |
| `price`       | Price per unit                |
| `region`      | Geographic region of customer |

---

## 💻 Tools Used

* **MySQL** – Data cleaning, aggregation, and cohort analysis
* **Tableau** – KPI cards, bar charts, and heatmaps
* **CSV/Excel** – Data storage and export

---

## 🧼 Step 1: Data Preparation

**Database & Table Creation**

```sql
CREATE DATABASE clv_project;
USE clv_project;

CREATE TABLE orders (
    order_id INT,
    customer_id INT,
    order_date DATE,
    quantity INT,
    price DECIMAL(10,2),
    region VARCHAR(100)
);
```

**Lifetime Value Calculation**

```sql
SELECT 
    customer_id,
    SUM(quantity * price) AS lifetime_value,
    MIN(order_date) AS first_order,
    MAX(order_date) AS last_order,
    COUNT(DISTINCT order_id) AS total_orders
FROM orders
GROUP BY customer_id
ORDER BY lifetime_value DESC;
```

**Cohort Retention Table**

```sql
WITH first_orders AS (
    SELECT customer_id, MIN(order_date) AS first_order_date
    FROM orders
    GROUP BY customer_id
),
cohort_activity AS (
    SELECT o.customer_id,
           DATE_FORMAT(f.first_order_date, '%Y-%m') AS cohort_month,
           DATE_FORMAT(o.order_date, '%Y-%m') AS active_month
    FROM orders o
    JOIN first_orders f ON o.customer_id = f.customer_id
)
SELECT cohort_month, active_month, COUNT(DISTINCT customer_id) AS active_customers
FROM cohort_activity
GROUP BY cohort_month, active_month
ORDER BY cohort_month, active_month;
```

---

## 🔍 Step 2: Data Analysis & Visualization

**KPI Cards**

* Total Customers: 100
* Max CLV: 6,793
* Average CLV: 3,217

**Bar Chart**

* Top 10 customers ranked by lifetime value

**Cohort Heatmap**

* Retention over time by cohort month
* Drop-off patterns visible after months 3–4

---

## 📌 Step 3: Key Insights

✅ **High-Value Customers**
A small group of customers contribute disproportionately to total revenue.

⚠️ **Retention Drop**
Significant customer drop-off after 3–4 months in most cohorts.

🔄 **Strong Cohorts**
Certain early cohorts maintain higher retention — potential loyalty drivers worth replicating.

---

## 📈 Step 4: Dashboard

The Tableau dashboard features:

* KPI section for quick performance overview
* Top customers bar chart
* Cohort retention heatmap to visualize long-term engagement


## 📸 Dashboard Preview

![Tableau Dashboard](Tableau%20Dashboard.png)


---

## 📝 Step 5: Recommendations

**Retention Strategy**

* Launch re-engagement campaigns at months 2–3 to prevent churn.

**Loyalty Programs**

* Incentivize repeat purchases for top cohorts with special offers.

**Data Monitoring**

* Track retention monthly to quickly detect negative shifts.

---

## 📂 Files Included

* `clv_analysis.sql` – SQL queries for CLV & cohort analysis
* `orders.csv` – Sample transaction dataset
* `Tableau Dashboard.png` – Tableau dashboard visual
* `README.md` – Full project write-up

---

