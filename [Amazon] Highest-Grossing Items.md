# Amazon Top 2 Highest-Grossing Products by Category (2022)

## 📖 Problem Description

You are given a table containing data on Amazon customers and their spending on products across different categories.  
Your task is to write a SQL query to **identify the top two highest-grossing products within each category in the year 2022**.

The output should include:
- `category`
- `product`
- `total_spend`

---

## 📦 Table: `product_spend`

| Column Name        | Type      |
|---------------------|-----------|
| category           | string    |
| product           | string    |
| user_id           | integer   |
| spend            | decimal   |
| transaction_date | timestamp |

---

## 💾 Example Input

| category    | product           | user_id | spend   | transaction_date       |
|-------------|-------------------|---------|---------|------------------------|
| appliance   | refrigerator      | 165     | 246.00 | 12/26/2021 12:00:00  |
| appliance   | refrigerator      | 123     | 299.99 | 03/02/2022 12:00:00  |
| appliance   | washing machine   | 123     | 219.80 | 03/02/2022 12:00:00  |
| electronics | vacuum            | 178     | 152.00 | 04/05/2022 12:00:00  |
| electronics | wireless headset  | 156     | 249.90 | 07/08/2022 12:00:00  |
| electronics | vacuum            | 145     | 189.00 | 07/15/2022 12:00:00  |

---

## ✅ Example Output

| category    | product           | total_spend |
|-------------|-------------------|-------------|
| appliance   | refrigerator      | 299.99     |
| appliance   | washing machine   | 219.80     |
| electronics | vacuum            | 341.00     |
| electronics | wireless headset  | 249.90     |

---

## 🛠️ Explanation

- **appliance**:
    - refrigerator → 299.99
    - washing machine → 219.80

- **electronics**:
    - vacuum → 152.00 + 189.00 = 341.00
    - wireless headset → 249.90

---

## 💡 SQL Solution

```sql
with cte as (SELECT category,product,sum(spend) as spend, ROW_NUMBER() over (partition by category order 
by sum(spend) desc ) as rank  FROM product_spend where extract (year from transaction_date)=2022 group by 1,2 )

select category,product,spend as total_spend from cte where rank in (1,2)
; 
