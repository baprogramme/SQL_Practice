# Viewership Summary Query

## Problem Statement

Given a `viewership` table that records user activity categorized by device types (`laptop`, `tablet`, and `phone`), write a SQL query that calculates the total number of views for:
- Laptops (`laptop_views`)
- Mobile devices (`mobile_views`), where **mobile** is defined as the combination of `tablet` and `phone`.

The output should contain two columns:
- `laptop_views`: Total views from laptops
- `mobile_views`: Total views from tablets and phones combined


---

## Table Schema

**viewership**

| Column Name | Type    |
|-------------|---------|
| user_id     | integer |
| device_type | string  |
| view_time   | timestamp |

---

## Example Input

| user_id | device_type | view_time           |
|---------|-------------|---------------------|
| 123     | tablet      | 01/02/2022 00:00:00 |
| 125     | laptop      | 01/07/2022 00:00:00 |
| 128     | laptop      | 02/09/2022 00:00:00 |
| 129     | phone       | 02/09/2022 00:00:00 |
| 145     | tablet      | 02/24/2022 00:00:00 |

---

## Expected Output

| laptop_views | mobile_views |
|--------------|--------------|
| 2            | 3            |

---

## SQL Query

```sql
with cte as (select 
case
when device_type='laptop_views' then views end as laptop_views,
case
when device_type='mobile_views' then views end as mobile_views

from (select 
CASe 
when device_type='phone' then 'mobile_views'
when device_type='tablet' then 'mobile_views'
else
'laptop_views' end as device_type
, sum(views) as views from (SELECT device_type,count
(distinct user_id) as views FROM viewership 
group by device_type) as temp group by 1)temp2 order by 1,2 desc)

select max(laptop_views) as laptop_views,max(mobile_views) as mobile_views from cte;
