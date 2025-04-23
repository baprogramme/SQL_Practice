# Click-Through Rate (CTR) Calculation for Facebook App Analytics

## Overview

This project demonstrates how to calculate the **Click-Through Rate (CTR)** for each app in the year 2022 using an `events` table from Facebook app analytics data.

### Definition

CTR is calculated as:

CTR (%) = 100.0 * (Number of clicks) / (Number of impressions)

## Important Notes

- The calculation uses `100.0` instead of `100` to ensure floating point division.
- The output is rounded to **2 decimal places**.
- Only events from **2022** are considered.

---

## Schema

### `events` Table

| Column Name | Type     | Description                   |
|-------------|----------|-------------------------------|
| app_id      | integer  | Unique identifier for the app |
| event_type  | string   | Type of event (`impression` or `click`) |
| timestamp   | datetime | Time the event occurred       |

---

## Sample Input

| app_id | event_type | timestamp           |
|--------|------------|---------------------|
| 123    | impression | 2022-07-18 11:36:12 |
| 123    | impression | 2022-07-18 11:37:12 |
| 123    | click      | 2022-07-18 11:37:42 |
| 234    | impression | 2022-07-18 14:15:12 |
| 234    | click      | 2022-07-18 14:16:12 |

---

## Expected Output

| app_id | ctr   |
|--------|-------|
| 123    | 50.00 |
| 234    | 100.00|

---

## SQL Query

```sql
with cte as (SELECT app_id,count(case when event_type='impression' then 1 end ) as impression,
count(case when event_type='click' then 1 end ) as click FROM events where 
extract(year from (cast(timestamp as timestamp)))=2022 group by 1)

select app_id,Round(((cast(click as numeric)/cast(impression as numeric))*100.0),2) from cte; 



