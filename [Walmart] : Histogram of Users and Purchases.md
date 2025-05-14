## 🛒 Walmart User Transactions Analysis

**Problem Statement:**

Assume you're given a table on **Walmart user transactions**. Based on their most recent transaction date, write a query that retrieves the **users along with the number of products they bought**.

**Goal:** Output the user's **most recent transaction date**, **user ID**, and the **number of products**, sorted in **chronological order** by the transaction date.

---

### 📊 Table: `user_transactions`

| Column Name       | Type      |
|-------------------|-----------|
| `product_id`      | integer   |
| `user_id`         | integer   |
| `spend`           | decimal   |
| `transaction_date`| timestamp |

---

### 🔽 Example Input:

| product_id | user_id | spend  | transaction_date       |
|------------|---------|--------|------------------------|
| 3673       | 123     | 68.90  | 07/08/2022 12:00:00    |
| 9623       | 123     | 274.10 | 07/08/2022 12:00:00    |
| 1467       | 115     | 19.90  | 07/08/2022 12:00:00    |
| 2513       | 159     | 25.00  | 07/08/2022 12:00:00    |
| 1452       | 159     | 74.50  | 07/10/2022 12:00:00    |

---

### ✅ Example Output:

| transaction_date       | user_id | purchase_count |
|------------------------|---------|----------------|
| 07/08/2022 12:00:00    | 115     | 1              |
| 07/08/2022 12:00:00    | 123     | 2              |
| 07/10/2022 12:00:00    | 159     | 1              |

---

### 🧠 Explanation:

- User **123** made **2 purchases** on **07/08/2022** → most recent = 07/08
- User **115** made **1 purchase** on **07/08/2022**
- User **159** had 2 transactions, but their **most recent** was on **07/10/2022**, so we count only that purchase.

The dataset you are querying against may have different input & output – this is just an example!

``` sql

with cte1 as (
select product_id,user_id,spend,transaction_date, RANK() over(partition by user_id order by transaction_date desc ) as rnk
from user_transactions )

select transaction_date,user_id,count(product_id) as purchase_count from cte1 where rnk=1 group by 1,2 order by 1;
