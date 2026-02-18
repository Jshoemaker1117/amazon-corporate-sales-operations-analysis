#  Amazon Corporate Sales & Operations Analysis (SQL)

## Project Overview
This project simulates a corporate-level business intelligence analysis for an Amazon-style retail operation. Using SQLite, I analyzed revenue, profitability, shipping costs, discount impact, return rates, and average order value (AOV) across product categories and regions.

---

##  Business Objectives

- Identify most and least profitable regions  
- Analyze margin performance by product category  
- Evaluate return rates and operational impact  
- Assess shipping cost as % of revenue  
- Analyze discount impact on profitability  
- Evaluate average order value (AOV)  

---

##  Key Insights

- **Highest Margin Category:** Beauty (20.48%)  
- **Lowest Margin Categories:** Grocery (-22.65%), Books (-28.15%)  
- **Highest Return Rate:** Fashion (13.79%)  
- **Highest AOV:** Electronics ($164.26)  
- **Books had lowest AOV ($19.24)** and highest shipping % of revenue (31.84%)

### Executive Interpretation
Books demonstrated structural margin weakness driven by low average order value and high shipping burden. Grocery losses were primarily influenced by discounting strategy. Electronics showed strong unit economics with high AOV and controlled shipping impact.

---

## Business Recommendations

- Reassess pricing and shipping structure for Books & Grocery  
- Implement bundling strategy to increase AOV in low-margin categories  
- Investigate high return rate in Fashion (size/quality control review)  
- Protect and scale high-margin categories (Beauty, Fashion)  

---

##  Tools Used

- SQLite  
- DB Browser for SQLite  
- SQL (JOINs, Aggregations, Margin Calculations)  

---

##  Sample SQL Logic Used

### Net Margin by Category
```sql
SELECT 
    p.category,
    ROUND(SUM(o.revenue), 2) AS revenue,
    ROUND(SUM(o.net_profit), 2) AS net_profit,
    ROUND(100.0 * SUM(o.net_profit) / NULLIF(SUM(o.revenue),0), 2) AS net_margin_pct
FROM orders o
JOIN products p
    ON o.product_id = p.product_id
GROUP BY p.category
ORDER BY net_margin_pct DESC;
```

### Return Rate by Category
```sql
SELECT 
    p.category,
    ROUND(AVG(o.returned) * 100, 2) AS return_rate_pct
FROM orders o
JOIN products p
    ON o.product_id = p.product_id
GROUP BY p.category
ORDER BY return_rate_pct DESC;
```

### Shipping Cost as % of Revenue
```sql
SELECT 
    p.category,
    ROUND(100.0 * AVG(o.shipping_cost) / NULLIF(AVG(o.revenue),0), 2) AS shipping_pct_of_revenue
FROM orders o
JOIN products p
    ON o.product_id = p.product_id
GROUP BY p.category
ORDER BY shipping_pct_of_revenue DESC;
```

