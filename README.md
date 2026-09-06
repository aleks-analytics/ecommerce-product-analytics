# E-Commerce Product & Unit Economics Analytics

[English Version](README.md) | [Русская версия](README_RU.md)

---

### Project Overview
This project examines transaction and behavioral event logs from an e-commerce platform across three relational tables (`orders`, `users`, `user_actions`)

The analysis tracks the full customer lifecycle: from the first app open and checkout funnel steps to repeat purchases, cohort revenue dynamics, and long-term channel LTV

---

### Key Analytical Sections & SQL Pipeline

1. **Category Performance & Cancellation Rates**
   * Calculated net completed revenue, average order value (AOV), and return/cancellation rates per category
   * Electronics generates the highest AOV (~$345), while Home & Living averages ~$170, and apparel/cosmetics form the high-volume core at $60–100

2. **Regional VIP Spenders (Window Functions)**
   * Ranked customers within each country by cumulative spend using `DENSE_RANK() OVER (PARTITION BY country ORDER BY ... DESC)`
   * The US market exhibits the highest concentration of high-tier spenders ($1,600+), while top customers across European markets maintain consistent spending levels ($1,250–1,400)

3. **Repurchase Frequency & Days to Return (`LEAD`)**
   * Isolated repeat customers (2+ orders) and tracked consecutive order intervals using `julianday(LEAD(order_date) OVER (...)) - julianday(order_date)`
   * Identified a strict 12-day average purchase cycle, providing a concrete timeline for CRM retention triggers and personalized push notifications

4. **Cohort Revenue Dynamics**
   * Segmented users into registration cohorts (`ym_reg`) and tracked monthly revenue with cumulative window aggregations
   * Identified a recurring behavioral pattern: customer spend peaks in their second month on the platform (~$52–53k) rather than their registration month, highlighting the impact of initial onboarding

5. **In-App Checkout Funnel (Funnel CR)**
   * Built step-by-step conversion rates across five core events: `app_open` ➔ `product_view` ➔ `add_to_cart` ➔ `checkout_start` ➔ `payment_success`
   * Pinpointed the primary conversion bottleneck at the product card: 28% of users drop off before adding an item to their cart, while users who begin checkout convert at 88%

6. **Acquisition Channel Unit Economics & LTV**
   * Evaluated marketing channels by initial conversion (`cr1`), repeat purchase rate, and overall average LTV per acquired user to guide marketing budget allocation

---

### Tech Stack
* Python (Pandas, PandasSQL, Seaborn, Matplotlib)
* SQL (Multi-table joins, CTEs, Window functions `DENSE_RANK`, `LEAD`, cumulative `SUM() OVER`, conditional aggregation)

---

### Quickstart
```bash
git clone https://github.com/aleks-analytics/ecommerce-product-analytics.git
cd ecommerce-product-analytics
pip install pandas pandasql seaborn matplotlib
jupyter notebook
