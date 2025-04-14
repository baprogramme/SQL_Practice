# SQL Challenge: Find Facebook Pages with Zero Likes

## Problem Statement

You are given two tables containing data about Facebook Pages and their respective likes. Write a SQL query to return the IDs of the Facebook pages that have **zero likes**. The output should be sorted in **ascending** order based on the `page_id`.

## Table Schemas

### `pages` Table:

| Column Name | Type     |
|-------------|----------|
| page_id     | integer  |
| page_name   | varchar  |

### `page_likes` Table:

| Column Name | Type     |
|-------------|----------|
| user_id     | integer  |
| page_id     | integer  |
| liked_date  | datetime |

## Example Input

### `pages` Table:

| page_id | page_name              |
|---------|------------------------|
| 20001   | SQL Solutions          |
| 20045   | Brain Exercises        |
| 20701   | Tips for Data Analysts |

### `page_likes` Table:

| user_id | page_id | liked_date          |
|---------|---------|---------------------|
| 111     | 20001   | 04/08/2022 00:00:00 |
| 121     | 20045   | 03/12/2022 00:00:00 |
| 156     | 20001   | 07/25/2022 00:00:00 |

## Example Output

| page_id |
|---------|
| 20701   |

## SQL Solution

```sql
with cte as (select user_id,page_id,liked_date from page_likes )

select a.page_id from pages a left join cte b on a.page_id=b.page_id where b.page_id is null; 
