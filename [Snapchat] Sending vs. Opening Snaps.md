# Snapchat Activity Breakdown Analysis

## 📄 Problem Statement

Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent **sending vs. opening snaps as a percentage** of total time spent on these activities, grouped by **age group**. Round the percentage to **2 decimal places** in the output.

---

## 📋 Requirements

- Calculate the following percentages:
    - `time spent sending / (time spent sending + time spent opening)`
    - `time spent opening / (time spent sending + time spent opening)`

- To avoid integer division in percentages, multiply by **100.0** (not just 100).
  
---

## 🗃️ Table Structures

### activities Table

| Column Name    | Type     |
|---------------|----------|
| activity_id  | integer  |
| user_id      | integer  |
| activity_type| string ('send', 'open', 'chat') |
| time_spent   | float    |
| activity_date| datetime |

---

### age_breakdown Table

| Column Name | Type     |
|-------------|----------|
| user_id    | integer  |
| age_bucket | string ('21-25', '26-30', '31-35') |

---

## 📥 Example Input

### activities

| activity_id | user_id | activity_type | time_spent | activity_date        |
|-------------|---------|---------------|------------|----------------------|
| 7274       | 123     | open          | 4.50      | 06/22/2022 12:00:00 |
| 2425       | 123     | send          | 3.50      | 06/22/2022 12:00:00 |
| 1413       | 456     | send          | 5.67      | 06/23/2022 12:00:00 |
| 1414       | 789     | chat          | 11.00     | 06/25/2022 12:00:00 |
| 2536       | 456     | open          | 3.00      | 06/25/2022 12:00:00 |

### age_breakdown

| user_id | age_bucket |
|---------|------------|
| 123     | 31-35     |
| 456     | 26-30     |
| 789     | 21-25     |

---

## 📤 Example Output

| age_bucket | send_perc | open_perc |
|------------|-----------|-----------|
| 26-30     | 65.40     | 34.60     |
| 31-35     | 43.75     | 56.25     |

---

## 💡 Explanation

Using the age bucket **26-30** as an example:

- Total time spent sending: `5.67`
- Total time spent opening: `3.00`

Percentage of time spent sending: 5.67 / (5.67 + 3.00) = 0.65396 → 65.40%


Percentage of time spent opening: 3.00 / (5.67 + 3.00) = 0.34604 → 34.60%


---

## 💻 SQL Query

```sql

with cte as (SELECT b.age_bucket,
sum(case when activity_type='send' then time_spent else 0 end) as time_spent_sending,
sum(case when activity_type='open' then time_spent else 0 end) as time_spent_opening

FROM activities a inner join age_breakdown b 
on a.user_id=b.user_id group by 1 )

select age_bucket,Round(((time_spent_sending)/(time_spent_sending+time_spent_opening))*100.0,2) as send_perc,
Round(((time_spent_opening)/(time_spent_sending+time_spent_opening))*100.0,2) as open_perc from cte ; 





