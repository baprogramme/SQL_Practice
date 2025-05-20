# 📊 Year-on-Year Spend Growth Analysis

## Problem Statement

Assume you're given a table containing information about Wayfair user transactions for different products.  
Write a query to calculate the **year-on-year (YoY) growth rate** for the total spend of each product, grouping the results by `product_id`.

The output should include:
- Year (in ascending order)
- Product ID
- Current year's total spend
- Previous year's total spend
- YoY growth rate percentage (rounded to 2 decimal places)

---

## 🧾 Table Schema

**Table Name**: `user_transactions`

| Column Name       | Type      |
|-------------------|-----------|
| transaction_id    | integer   |
| product_id        | integer   |
| spend             | decimal   |
| transaction_date  | datetime  |

---

## 🧪 Example Input

| transaction_id | product_id | spend   | transaction_date      |
|----------------|------------|---------|------------------------|
| 1341           | 123424     | 1500.60 | 12/31/2019 12:00:00   |
| 1423           | 123424     | 1000.20 | 12/31/2020 12:00:00   |
| 1623           | 123424     | 1246.44 | 12/31/2021 12:00:00   |
| 1322           | 123424     | 2145.32 | 12/31/2022 12:00:00   |

---

## ✅ Expected Output

| year | product_id | curr_year_spend | prev_year_spend | yoy_rate |
|------|------------|------------------|------------------|----------|
| 2019 | 123424     | 1500.60          | NULL             | NULL     |
| 2020 | 123424     | 1000.20          | 1500.60          | -33.35   |
| 2021 | 123424     | 1246.44          | 1000.20          | 24.62    |
| 2022 | 123424     | 2145.32          | 1246.44          | 72.12    |

---

## 💡 Explanation

- In **2019**, there is no previous year's data, so `prev_year_spend` and `yoy_rate` are NULL.
- In **2020**, the spend is compared to 2019, resulting in a **-33.35%** drop.
- In **2021**, spend increases to 1246.44, compared to 1000.20 in 2020 (**24.62% growth**).
- In **2022**, the increase is more significant at **72.12% growth**.




---

> ⚠️ Note:  
> The dataset you are querying against may have different input & output - this is just an example!



``` sql


/* with cte as (SELECT extract(year from transaction_date) as year,
product_id,sum(spend) as curr_year_spend,row_number()
over (partition by product_id order by extract(year from transaction_date)) as rnk
FROM user_transactions 
group by 1,2 order by 2,1), cte2 as (

select year+1 as previous_year,product_id,curr_year_spend as prev_year_spend from cte order by 2,1)


select a.year,a.product_id,a.curr_year_spend,b.prev_year_spend as prev_year_spend,
case when b.prev_year_spend is null then Null else 
round(((a.curr_year_spend-b.prev_year_spend)/b.prev_year_spend)*100.0,2) end as yoy_rate


from cte a left join cte2 b on a.year=b.previous_year and 
a.product_id=b.product_id;   */


with cte as (select Extract (year from transaction_date) as year, product_id,sum(case when spend<>0 then spend else NULL end) as curr_year_spend, rank() over 
(partition by product_id order by Extract (year from transaction_date)) as rnk from 
user_transactions group by Extract (year from transaction_date), product_id), cte2 as (

select year,product_id,curr_year_spend,
LAG(curr_year_spend) over (partition by product_id order 
by year ) as prev_year_spend from cte ORDER BY product_id, year)

select *, case when prev_year_spend is not null then round(100*(1.0*(curr_year_spend-prev_year_spend)/prev_year_spend),2) else NULL end  as yoy_rate

from cte2
