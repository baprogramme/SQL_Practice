## JPMorgan Chase Credit Card Launch Analysis

Your team at **JPMorgan Chase** is soon launching a new credit card. You are asked to estimate how many cards you'll issue in the first month.

Before you can answer this question, you want to first get some perspective on how well new credit card launches typically do in their first month.

### 💼 Task
Write a query that outputs:
- The name of the credit card
- The number of cards issued in its **launch month**

> The launch month is defined as the **earliest record** in the `monthly_cards_issued` table for a given card.

Sort the results by the **biggest issued amount** first.

---

### 🧾 monthly_cards_issued Table Schema

| Column Name   | Type     | Description                      |
|---------------|----------|----------------------------------|
| issue_month   | integer  | Month the card was issued        |
| issue_year    | integer  | Year the card was issued         |
| card_name     | string   | Name of the credit card          |
| issued_amount | integer  | Number of cards issued           |

---

### 📘 Example Input

| issue_month | issue_year | card_name             | issued_amount |
|-------------|------------|-----------------------|----------------|
| 1           | 2021       | Chase Sapphire Reserve| 170000         |
| 2           | 2021       | Chase Sapphire Reserve| 175000         |
| 3           | 2021       | Chase Sapphire Reserve| 180000         |
| 3           | 2021       | Chase Freedom Flex    | 65000          |
| 4           | 2021       | Chase Freedom Flex    | 70000          |

---

### ✅ Example Output

| card_name             | issued_amount |
|-----------------------|----------------|
| Chase Sapphire Reserve| 170000         |
| Chase Freedom Flex    | 65000          |

---

### 📌 Explanation

- **Chase Sapphire Reserve** card was launched in **January 2021** with 170,000 cards issued.
- **Chase Freedom Flex** card was launched in **March 2021** with 65,000 cards issued.

---

📝 *The dataset you are querying against may have different input & output - this is just an example!*


``` sql

with cte as (





select card_name,issue_year,issue_month,issued_amount, rank() over(partition by card_name
 order by issue_year,issue_month) as rnk from monthly_cards_issued)
 

 
 select card_name,issued_amount from cte where rnk=1 order by 2 desc
