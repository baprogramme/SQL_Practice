# Sum of Odd and Even Numbered Measurements

## Problem Statement

Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns.

### Definition:
- Within a day, measurements taken at 1st, 3rd, and 5th times are considered **odd-numbered** measurements.
- Measurements taken at 2nd, 4th, and 6th times are considered **even-numbered** measurements.



---

## Tables

### measurements

| Column Name        | Type      |
|--------------------|-----------|
| measurement_id     | integer   |
| measurement_value  | decimal   |
| measurement_time   | datetime  |

### Example Input

| measurement_id | measurement_value | measurement_time       |
|----------------|-------------------|-------------------------|
| 131233         | 1109.51           | 07/10/2022 09:00:00     |
| 135211         | 1662.74           | 07/10/2022 11:00:00     |
| 523542         | 1246.24           | 07/10/2022 13:15:00     |
| 143562         | 1124.50           | 07/11/2022 15:00:00     |
| 346462         | 1234.14           | 07/11/2022 16:45:00     |

---

## Example Output

| measurement_day      | odd_sum | even_sum |
|----------------------|---------|----------|
| 07/10/2022 00:00:00  | 2355.75 | 1662.74  |
| 07/11/2022 00:00:00  | 1124.50 | 1234.14  |

### Explanation

- On **07/10/2022**, the 1st and 3rd measurements are 1109.51 and 1246.24 (odd), summing to **2355.75**, and the 2nd measurement is 1662.74 (even).
- On **07/11/2022**, the 1st measurement is 1124.50 (odd) and the 2nd is 1234.14 (even).

---

## SQL Solution
```sql 
with cte as (
SELECT 
    measurement_id,
    measurement_value,
    DATE(measurement_time) AS measurement_day,
    ROW_NUMBER() OVER (
        PARTITION BY DATE(measurement_time) 
        ORDER BY measurement_time
    ) AS rank
FROM 
    measurements )

select measurement_day, sum( case when ( rank % 2 ) !=0 then measurement_value end ) as odd_sum,
sum(case when ( rank % 2 ) =0 then measurement_value end) as even_sum from cte group by 1 order by 1;

---

## Tags
SQL, Window Functions, Aggregation
