# 📊 SQL Query: Monthly Active Users (MAUs) for July 2022

## 📝 Problem Statement

Assume you're given a table containing information on Facebook user actions. Write a query to obtain the number of **monthly active users (MAUs)** in **July 2022**, including the **month in numerical format** (e.g., `1`, `2`, `3`, ...).

### 💡 Hint:
An **active user** is defined as a user who has performed actions such as `'sign-in'`, `'like'`, or `'comment'` **in both the current month and the previous month**.

---

## 📘 Table: `user_actions`

| Column Name | Type     | Description                               |
|-------------|----------|-------------------------------------------|
| `user_id`   | integer  | ID of the user                            |
| `event_id`  | integer  | ID of the event                           |
| `event_type`| string   | Type of event: `"sign-in"`, `"like"`, `"comment"` |
| `event_date`| datetime | Timestamp of the event                    |

---

## 📥 Example Input (`user_actions`)

| user_id | event_id | event_type | event_date          |
|---------|----------|-------------|----------------------|
| 445     | 7765     | sign-in     | 2022-05-31 12:00:00 |
| 742     | 6458     | sign-in     | 2022-06-03 12:00:00 |
| 445     | 3634     | like        | 2022-06-05 12:00:00 |
| 742     | 1374     | comment     | 2022-06-05 12:00:00 |
| 648     | 3124     | like        | 2022-06-18 12:00:00 |

---

## 📤 Example Output (for June 2022)

| month | monthly_active_users |
|--------|-----------------------|
| 6      | 1                     |

### Explanation:
In **June 2022**, there was only one monthly active user (`user_id = 445`) who had activity in **both** May and June.

> ⚠️ Please adapt your solution to calculate MAUs for **July 2022**.

---

## 🧠 Goal:
- Write a SQL query to determine the number of **monthly active users in July 2022**.
- Ensure output includes:
  - `month`: numerical value (e.g., `7`)
  - `monthly_active_users`: count of distinct active users meeting the criteria
 
``` sql

/* with cte as (
SELECT user_id,event_id,event_type,EXTRACT
(month from event_date) as month FROM user_actions where 
extract(month from date(event_date))<=7
and extract(month from date(event_date))>=6
and extract(year from date(event_date))=2022), cte2 as (

select * from cte where event_type='sign-in' and month in (6,7)), cte3 as (

select * from cte where event_type in ('like','comment') and month in (6,7)),
 cte4 as (

select a.user_id as user_id,
a.event_id as sign_in_event_id,
a.month as sign_in_month,
b.event_id as like_or_comment_event_id,
b.month as like_or_comment_month 
from cte2 a inner join cte3 b
on a.user_id=b.user_id and a.month=b.month ), cte5 as(

select user_id,count(distinct sign_in_month) from cte4  group by 1 having count(distinct sign_in_month)>1)

select max(a.sign_in_month) as month, count(b.user_id) as monthly_active_users  from cte4 a inner join cte5 b 

on a.user_id=b.user_id;  */


with cte as (
select * from user_actions where event_type in ('sign-in','like','comment') 
and  Extract (year from event_date)=2022 
and Extract (month from event_date) in (6, 7) order by user_id, event_date), cte2 as (

select user_id,Extract (month from event_date) as month, dense_rank() 
over(partition by user_id order by Extract (month from event_date)) as rnk from cte)

select  month, count(distinct user_id) as monthly_active_users from cte2 where rnk =2 group by 1
;
