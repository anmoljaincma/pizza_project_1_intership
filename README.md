### pizza_project_1_intership
It is my Data Analytics Internship Project Number 1 titled "pizza sales" in UNIFIED MENTOR.
# 🍕 Plato’s Pizza Data Analysis – Project Scope & Objectives

---

## 📌 Project Title  
**The Pizza Challenge – Sales & Operations Analysis for Plato’s Pizza**

---

## 🎯 Objective  
Analyze one year of transactional data from Plato’s Pizza to uncover trends in pizza sales, customer behavior, and restaurant capacity usage. The goal is to provide actionable insights that help improve revenue, operational efficiency, and customer experience.

---

## 📦 Scope of Analysis (In-Scope)
- Identify busiest days and times of the week  
- Analyze pizza sales by type, size, and quantity  
- Determine peak hours and pizza demand during those times  
- Calculate average order value (AOV)  
- Estimate seating/table utilization based on customer volume  
- Explore seasonal or monthly sales trends (if applicable)

---

## 🚫 Out of Scope (Excluded from This Project)
- Ingredient inventory and cost optimization  
- Delivery logistics or time tracking  
- Employee performance or shift scheduling  
- Customer demographic profiling (if not available in data)

---

## ❓ Key Questions to Answer
1. What days and times do we tend to be busiest?  
2. How many pizzas are we making during peak periods?  
3. What are our best and worst-selling pizzas?  
4. What’s our average order value?  
5. How well are we utilizing our seating capacity?  
6. Are there seasonal or monthly trends in sales?  
7. Are there opportunities to increase sales through promotions or combos?

---

## 📏 Key Metrics to Track
- Number of orders  
- Quantity of pizzas sold  
- Revenue per order  
- Pizza type/size sales breakdown  
- Timestamps (for time-based analysis)  
- Estimated customer count (for seating analysis)  
- Table and seat utilization (15 tables, 60 seats)

---

## 📁 Notes
- Data covers one year of transactions  
- Assumptions (e.g., number of people per order) may be used if customer count is not provided  
- Data cleaning will be the next step before analysis begins

# 📊 Key KPIs for Pizza Sales Analysis (with SQL Queries)
### Note: All screenshots of result in 'INT Project 1 - Plato’s Pizza Data Analysis Report' file

## 1. 🧾 Total Revenue
Definition: Total money earned from all orders  
 Formula: SUM(total_price)  
```sql
SELECT 
round(SUM(total_price),2) AS total_revenue
FROM pizza_sales;
```

## 2. 💰 Average Order Value (AOV)
Definition: Average revenue generated per order  
 Formula: SUM(total_price) / COUNT(DISTINCT order_id)
```sql
SELECT 
ROUND(SUM(total_price) * 1.0 / COUNT(DISTINCT order_id), 2) AS avg_order_value
FROM pizza_sales;
```


## 3. 🍕 Total Pizzas Sold
Definition: Total number of pizza units sold  
 Formula: SUM(quantity)
```sql
SELECT 
  SUM(quantity) AS total_pizzas_sold
FROM pizza_sales;
```

## 4. 🏆 Best-Selling Pizzas(Top 5)
Definition: Pizzas with the highest quantity sold  
 Formula: SUM(quantity) grouped by pizza_name
```sql
SELECT TOP 5
  pizza_name,
  SUM(quantity) AS total_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_sold DESC;
```

## 5. ❌ Worst-Selling Pizzas(Bottom 5)
Definition: Pizzas with the lowest quantity sold
```sql
SELECT TOP 5
  pizza_name,
  SUM(quantity) AS total_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_sold ASC;
```

## 6. ⏰ Peak Order Hour(TOP 5)
Definition: The hour of the day when most sales occur  
 Formula: Group by HOUR(order_time)
```sql
SELECT TOP 5
  DATEPART(HOUR, order_time) AS order_hour,
  round(SUM(total_price),2) AS hourly_revenue
FROM pizza_sales
GROUP BY DATEPART(HOUR, order_time)
ORDER BY hourly_revenue DESC;
```

## 7. 📅 Busiest Day of the Week
Definition: A Day with the highest total revenue  
 Formula: Group by weekday name
```sql
SELECT 
  DATENAME(WEEKDAY, CAST(order_date AS DATE)) AS day_name,
  round(SUM(total_price),2) AS daily_revenue
FROM pizza_sales
GROUP BY DATENAME(WEEKDAY, CAST(order_date AS DATE))
ORDER BY daily_revenue DESC;
```

## 8. 📐 Revenue by Pizza Size
Definition: Total revenue earned by each pizza size  
```sql
SELECT 
  pizza_size,
  round(SUM(total_price),2) AS revenue
FROM pizza_sales
GROUP BY pizza_size
ORDER BY revenue DESC;
```

## 9. 🥦 Revenue by Pizza Category
Definition: Revenue earned by Classic, Veggie, Supreme, and Chicken  
```sql
SELECT 
  pizza_category,
  SUM(total_price) AS revenue
FROM pizza_sales
GROUP BY pizza_category
ORDER BY revenue DESC;
```

## 10. 🪑 Estimated Seat Utilisation Rate
Customer Estimation Logic
We analysed 21,350 unique orders and 48,620 pizza line items. On average, each order consists of 2.28 pizza items.
Therefore, we estimate customer count based on the number of order_details_id, assuming each pizza item represents one person (≈ 1 pizza per person).
This results in an estimated 48,620 customers served over the year.
```sql
SELECT 
  ROUND(CAST(48620 AS FLOAT)/60, 2) AS total_full_seat_cycles,
  ROUND(CAST(48620 AS FLOAT) / 60 / 365, 2) AS avg_full_seat_cycles_per_day,
  ROUND(CAST(48620 AS FLOAT) / (365 * 60) * 100, 2) AS avg_daily_seat_utilization_percent
```
Seating Utilisation Insight:
Over the years, Plato’s Pizza served approximately 48,620 customers.
Given 60 seats and 365 days, this equates to a daily average seating utilisation of 222%, meaning each seat is used 2.2 times daily.
This reflects a healthy table turnover, especially during peak hours.

# Data Cleaning

1. Changed order_details_id, order_id, quantity, unit_price and total_price to their respective number formats(int and float) from default string formats.  
2. Changed order_date and order_time to respective date and time format from string format.  
3. Trimmed all cells in a different sheet, applying a formula:  


#### =ARRAYFORMULA(TRIM(pizza_sales!A1:L48621))























  
