# Amazon Shopping Spree Detection

## Problem Statement

In an effort to identify high-value customers, Amazon asked for your help to obtain data about users who go on **shopping sprees**.  
A **shopping spree** occurs when a user makes purchases on **3 or more consecutive days**.

Your task is to:
- List the **user IDs** who have gone on **at least 1 shopping spree** in **ascending order**.

---

## Table: `transactions`

| Column Name       | Type      |
|-------------------|-----------|
| `user_id`         | integer   |
| `amount`          | float     |
| `transaction_date`| timestamp |

---

## Example Input

### Table: `transactions`

| user_id | amount | transaction_date       |
|---------|--------|------------------------|
| 1       | 9.99   | 08/01/2022 10:00:00    |
| 1       | 55.00  | 08/17/2022 10:00:00    |
| 2       | 149.5  | 08/05/2022 10:00:00    |
| 2       | 4.89   | 08/06/2022 10:00:00    |
| 2       | 34.00  | 08/07/2022 10:00:00    |

---

## Example Output

| user_id |
|---------|
| 2       |

---

## Explanation

In this example, `user_id` 2 is the only one who has gone on a shopping spree.  
They made purchases on **3 consecutive days**:  
- 08/05/2022  
- 08/06/2022  
- 08/07/2022  

This qualifies as a shopping spree according to the problem definition.

---

> **Note:**  
> The dataset you are querying against may have different input & output – this is just an example!


```  sql

with cte1 as (SELECT distinct user_id, date(transaction_date) as trans_date FROM transactions), cte2 as (
select user_id,trans_date,trans_date-Interval '1 day'*row_number() over (partition by user_id order by trans_date) as grp
from cte1),ct3 as (
select user_id,count(*) from cte2 group by 1,grp having count(*)>=3)

select distinct user_id from ct3;

