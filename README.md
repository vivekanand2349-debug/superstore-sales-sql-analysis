# Superstore Sales SQL Analysis

A MySQL project that analyses a Superstore-style sales database to answer business questions about revenue, profit, customers, products and regions. It uses joins, subqueries, CTEs and window functions.

## Project Overview

The goal of this project is to practise end-to-end SQL analysis: designing a relational database, loading data, and writing queries that turn raw transactions into business insights. It complements my [Global Superstore Power BI Dashboard](https://github.com), which covers the visual side of the same theme.

## Tools Used

- MySQL 8.0
- MySQL Workbench

## Dataset

The data is **synthetic**. It was generated with the help of Claude (an AI assistant) and is not real company data.

| Table | Rows (approx.) | Description |
|---|---|---|
| `regions` | 5 | North, South, East, West, Central |
| `customers` | 50 | Customer name, segment, city, state, region |
| `products` | 30 | Category, sub-category, unit price, unit cost |
| `orders` | 400 | Order date, ship date, ship mode, status (2023-2024) |
| `order_items` | ~1,000 | Products in each order, quantity, discount |

## Database Design

`order_items` is a bridge table that links orders and products (many-to-many).
regions (1) ──< customers (1) ──< orders (1) ──< order_items >── (1) products

## Business Questions Answered

| # | Question | SQL concepts |
|---|---|---|
| 1 | Total revenue, cost, profit and profit margin % | Joins, aggregation |
| 2 | Monthly revenue trend (2023-2024) | `DATE_FORMAT`, `GROUP BY` |
| 3 | Revenue and profit by category and sub-category | Multi-table join, `GROUP BY` |
| 4 | Revenue and profit by region | 5-table join |
| 5 | Customers who have never placed an order | `LEFT JOIN`, `IS NULL`, `NOT EXISTS` |
| 6 | Products with sales above the average product sales | Subquery, `HAVING` |
| 7 | Customers with spend above the average customer spend | Subquery, `HAVING` |
| 8 | Top 5 customers per segment by revenue | CTE, `ROW_NUMBER()`, `PARTITION BY` |
| 9 | Month-over-month revenue growth % | CTE, `LAG()` |
| 10 | Rank products by revenue within each category | `RANK()`, `DENSE_RANK()` |
| 11 | Running total of monthly revenue | `SUM() OVER (ORDER BY ...)` |

Revenue is calculated as `quantity * unit_price * (1 - discount)`, and profit as revenue minus `quantity * unit_cost`. Cancelled orders are excluded from all revenue and profit figures.

## Key Findings

All figures exclude cancelled orders. Currency is shown in ₹ (the data is synthetic).

* **Overall Performance:** Total revenue was **₹18,555,647** (approx. ₹1.86 crore) with a total profit of **₹3,694,127**, yielding a solid **19.91% profit margin**. Year-over-year revenue grew strongly by **+23%**, moving from ₹8.33M in 2023 up to ₹10.23M in 2024.
* **Regional Breakdown:** The **West** region generated the highest overall revenue at **₹6.36M** (accounting for roughly 34% of the total dataset). However, the **Central** region proved to be the most efficient, achieving the highest profit margin at **22.78%**. The **North** region was the lowest performer, bringing in **₹1.75M** in revenue.
* **Category Performance:** **Technology** contributed the largest share of revenue at **₹10.71M** but operated on the narrowest category margin at **15.7%**. Conversely, **Office Supplies** brought in the lowest total revenue but pulled the highest profit margin at **30.19%**.
* **Sub-category Analysis:** **Computers** was the largest sub-category by value, generating **₹5.59M**, yet it dragged the lowest overall profit margin at **12.13%**. The highest margin came from **Furnishings**, sitting at **38.14%**.
* **Timeline Trends:** Monthly revenue peaked in **June 2024** at **₹1.31M** and recorded its lowest drop in **May 2023** at **₹0.34M**. The project queries revealed volatile MoM trends, with the largest positive growth leap hitting **+200%** in **June 2023**, and the sharpest contraction plunge reaching **-62%** in **November 2023**.
* **Customer Insights:** Out of 50 total registered customers, **8 customers never placed an order**, while **22 customers spent above the average customer benchmark**. Individual spending concentration was high; a single top customer in the *Home Office* segment (**Vikram Gupta**) accounted for **₹1.01M** in total purchases.
* **Product Insights:** Out of 30 distinct items, **8 products performed above average product sales**. The leading top-sellers across each respective product category were:
  * *Technology:* **Laptop 15 inch** (₹4.17M)
  * *Furniture:* **Conference Table** (₹2.32M)
  * *Office Supplies:* **Paper Shredder** (₹0.43M)
* **Profitability Check:** The SQL analysis confirmed that **zero products were net loss-making** after final customer discounts were applied. However, high-volume, low-margin item categories like *Computers* represent a high business sensitivity risk if subjected to aggressive promotional discounting strategies.

## Skills Demonstrated

- Relational database design with primary and foreign keys
- Multi-table joins (`INNER JOIN`, `LEFT JOIN`)
- Aggregation with `GROUP BY` and `HAVING`
- Subqueries and Common Table Expressions (CTEs)
- Window functions: `ROW_NUMBER`, `RANK`, `DENSE_RANK`, `LAG`, running totals
- Business metrics: revenue, profit, margin %, growth %

## Approach

I defined the business questions and wrote the queries myself, then used Claude to review my SQL, fix errors and learn alternative approaches such as CTEs and window functions. I ran all queries in MySQL Workbench and wrote up the findings from my own results.

## Author

**Vivek Anand**  
Bengaluru, India  
[LinkedIn] https://www.linkedin.com/in/vivek-anand-037a32ba/
