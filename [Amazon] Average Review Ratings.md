# Average Star Rating by Product and Month

## Problem Statement

Given the `reviews` table, write a SQL query to retrieve the **average star rating** for each product, **grouped by month**.  
The output should display:

- The month as a **numerical value** (`mth`)
- The `product_id`
- The **average star rating**, rounded to two decimal places

Sort the output by `mth` and then by `product_id`.



---

## Table Schema

**reviews**

| Column Name | Type     |
|-------------|----------|
| review_id   | integer  |
| user_id     | integer  |
| submit_date | datetime |
| product_id  | integer  |
| stars       | integer (1–5) |

---

## Example Input

| review_id | user_id | submit_date        | product_id | stars |
|-----------|---------|--------------------|------------|--------|
| 6171      | 123     | 06/08/2022 00:00:00 | 50001      | 4      |
| 7802      | 265     | 06/10/2022 00:00:00 | 69852      | 4      |
| 5293      | 362     | 06/18/2022 00:00:00 | 50001      | 3      |
| 6352      | 192     | 07/26/2022 00:00:00 | 69852      | 3      |
| 4517      | 981     | 07/05/2022 00:00:00 | 69852      | 2      |

---

## Example Output

| mth | product | avg_stars |
|-----|---------|-----------|
| 6   | 50001   | 3.50      |
| 6   | 69852   | 4.00      |
| 7   | 69852   | 2.50      |

---

## Explanation

- Product 50001 received two ratings in June (4 and 3), so the average is **(4+3)/2 = 3.5**.
- Product 69852 had one review in June and two in July, so the averages are **4.0** for June and **(3+2)/2 = 2.5** for July.

---

## SQL Solution (PostgreSQL)

```sql
SELECT Extract(month from cast(submit_date as timestamp)) as mth,
product_id as product,
ROUND(AVG(stars)::numeric, 2) as avg_stars from reviews  group by 1,2 order by 1;
