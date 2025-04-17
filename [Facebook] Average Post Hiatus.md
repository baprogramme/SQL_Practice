# Days Between First and Last Facebook Post in 2021

## 🧠 Problem Statement

Given a table of Facebook posts, for each user who posted **at least twice in 2021**, write a query to find the **number of days between each user’s first post of the year and last post of the year** (only considering posts from 2021).

## 📚 Table: `posts`

| Column Name   | Type      |
|---------------|-----------|
| user_id       | integer   |
| post_id       | integer   |
| post_content  | text      |
| post_date     | timestamp |

## 🧪 Example Input

| user_id | post_id | post_content | post_date            |
|---------|---------|--------------|-----------------------|
| 151652  | 599415  | Need a hug   | 2021-07-10 12:00:00   |
| 661093  | 624356  | Busy day     | 2021-07-29 13:00:00   |
| 004239  | 784254  | Happy 4th!   | 2021-07-04 11:00:00   |
| 661093  | 442560  | Sad movie    | 2021-07-08 14:00:00   |
| 151652  | 111766  | Travel soon  | 2021-07-12 19:00:00   |

## ✅ Example Output

| user_id | days_between |
|---------|---------------|
| 151652  | 2             |
| 661093  | 21            |

---

## 🛠️ SQL Query (PostgreSQL)

```sql
WITH cte AS (
  SELECT 
    user_id,
    MIN(post_date::date) AS min_date,
    MAX(post_date::date) AS max_date
  FROM posts where Extract(year from(post_date))=2021
  GROUP BY user_id
)
SELECT 
  user_id,
  (max_date - min_date) AS days_between
FROM cte where max_date<>min_date order by 2 desc;


