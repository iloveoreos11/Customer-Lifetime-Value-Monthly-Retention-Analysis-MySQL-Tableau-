
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


**1. Retention Strategy – Address Drop-Off at Months 2–3**  
- **Problem:** The cohort heatmap showed significant drop-off after 3–4 months.  
- **Recommendation:** Launch **re-engagement campaigns earlier (month 2)** to prevent churn.  
  - Offer **£5 discount vouchers** for customers inactive for 30 days.  
  - Send **“We miss you” emails** with personalised product bundles.  
- **Impact:** Could retain an extra **10–15% of at-risk customers**, raising overall retention and protecting recurring revenue.  

**2. Loyalty Programs – Incentivise High-Value Customers**  
- **Problem:** A small group of customers contributed disproportionately to revenue (max CLV: £6,793).  
- **Recommendation:**  
  - Introduce **tiered loyalty rewards** (e.g., free shipping after 5 orders, VIP early access to sales).  
  - Provide **personalised offers** for high-value segments based on past purchasing habits.  
- **Impact:** Increases repeat purchase frequency and strengthens customer lifetime value.  

**3. Cohort Replication – Learn from Strong Cohorts**  
- **Problem:** Some early cohorts maintained stronger long-term retention.  
- **Recommendation:** Analyse what drove their loyalty (e.g., seasonal promotions, acquisition channel, product mix) and replicate those strategies in future campaigns.  
- **Impact:** Scaling proven onboarding and marketing approaches could boost new cohort retention by **5–8%**.  

**4. Data Monitoring – Ongoing Tracking**  
- **Problem:** Retention issues are only visible after several months.  
- **Recommendation:** Automate a **monthly SQL + Tableau dashboard update** to monitor retention in real time.  
- **Impact:** Enables earlier interventions, cutting churn lag and sustaining retention improvements over time.  


---

## 📂 Files Included

* `clv_analysis.sql` – SQL queries for CLV & cohort analysis
* `orders.csv` – Sample transaction dataset
* `Tableau Dashboard.png` – Tableau dashboard visual
* `README.md` – Full project write-up

---

