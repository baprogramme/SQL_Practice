# 📨 Top Microsoft Teams Power Users – August 2022

## 📌 Problem Statement

Write a SQL query to identify the **top 2 Power Users** who sent the highest number of messages on Microsoft Teams in **August 2022**. 

Display the `sender_id` of these users along with the **total number of messages** they sent, sorted in **descending order** based on the message count.

### Assumptions:
- No two users have sent the same number of messages in August 2022 (i.e., no tie in ranking).
- You are working with a `messages` table with the following schema:

| Column Name | Type     |
|-------------|----------|
| message_id  | integer  |
| sender_id   | integer  |
| receiver_id | integer  |
| content     | varchar  |
| sent_date   | datetime |

---

## 📋 Sample Input

**messages table:**

| message_id | sender_id | receiver_id | content                   | sent_date           |
|------------|-----------|-------------|----------------------------|---------------------|
| 901        | 3601      | 4500        | You up?                    | 08/03/2022 00:00:00 |
| 902        | 4500      | 3601        | Only if you're buying      | 08/03/2022 00:00:00 |
| 743        | 3601      | 8752        | Let's take this offline    | 06/14/2022 00:00:00 |
| 922        | 3601      | 4500        | Get on the call            | 08/10/2022 00:00:00 |

---

## ✅ Expected Output

| sender_id | message_count |
|-----------|----------------|
| 3601      | 2              |
| 4500      | 1              |

---

## 🧠 SQL Query

```sql
select sender_id,count(distinct message_id) from messages where 
Extract(year from sent_date)=2022 and 
Extract(month from sent_date)=8 group by sender_id order by 2 desc limit 2;
