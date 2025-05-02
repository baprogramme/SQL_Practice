# Uber Transactions SQL Query

## Problem Description

Assume you are given the table below on Uber transactions made by users.  

Write a SQL query to obtain **the third transaction of every user**. Output the `user_id`, `spend`, and `transaction_date`.

---

### Table: `transactions`

| Column Name       | Type      |
|-------------------|-----------|
| user_id          | integer   |
| spend           | decimal   |
| transaction_date | timestamp |

---

### Example Input

| user_id | spend  | transaction_date        |
|---------|--------|-------------------------|
| 111     | 100.50 | 01/08/2022 12:00:00    |
| 111     | 55.00  | 01/10/2022 12:00:00    |
| 121     | 36.00  | 01/18/2022 12:00:00    |
| 145     | 24.99  | 01/26/2022 12:00:00    |
| 111     | 89.60  | 02/05/2022 12:00:00    |

---

### Example Output

| user_id | spend  | transaction_date        |
|---------|--------|-------------------------|
| 111     | 89.60  | 02/05/2022 12:00:00    |

---

### Explanation

For user `111`, the third transaction is `89.60` on `02/05/2022`.  
Users `121` and `145` have fewer than three transactions, so they are not included in the output.

The dataset you are querying against may have different input & output - this is just an example!

---

### SQL Query

```sql
SELECT user_id, spend, transaction_date
FROM (
    SELECT user_id, spend, transaction_date,
           ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY transaction_date) AS rn
    FROM transactions
) ranked
WHERE rn = 3;
