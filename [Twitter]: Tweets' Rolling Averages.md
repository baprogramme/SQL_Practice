# 3-Day Rolling Average of Tweets

## Problem Statement

Given a table of tweet data over a specified time period, calculate the **3-day rolling average** of tweets for each user. Output the **user ID**, **tweet date**, and **rolling averages rounded to 2 decimal places**.

---

### Notes

- A rolling average (also known as a **moving average** or **running mean**) is a time-series technique that examines trends in data over a specified period of time.
- In this case, we want to determine how the **tweet count** for each user changes over a **3-day period**.

---

### Table: `tweets`

| Column Name  | Type      |
|--------------|-----------|
| user_id     | integer   |
| tweet_date  | timestamp |
| tweet_count | integer   |

---

### Example Input

| user_id | tweet_date           | tweet_count |
|---------|----------------------|-------------|
| 111     | 06/01/2022 00:00:00 | 2           |
| 111     | 06/02/2022 00:00:00 | 1           |
| 111     | 06/03/2022 00:00:00 | 3           |
| 111     | 06/04/2022 00:00:00 | 4           |
| 111     | 06/05/2022 00:00:00 | 5           |

---

### Example Output

| user_id | tweet_date           | rolling_avg_3d |
|---------|----------------------|----------------|
| 111     | 06/01/2022 00:00:00 | 2.00          |
| 111     | 06/02/2022 00:00:00 | 1.50         |
| 111     | 06/03/2022 00:00:00 | 2.00         |
| 111     | 06/04/2022 00:00:00 | 2.67         |
| 111     | 06/05/2022 00:00:00 | 4.00         |

---

### Requirements

- Calculate the **rolling average** over the past 3 days (including the current day) for each user.
- Round the result to **2 decimal places**.
- Ensure the logic works even when the dataset has gaps or multiple users.

---

### Important Note

Effective **April 7th, 2023**, the problem statement, solution, and hints for this question have been revised.

---

### SQL Query 

```sql


/*select 
user_id,
tweet_date,
round(avg(tweet_count) over(partition by user_id order by tweet_date	
Rows between 2 preceding and current row ),2) as rolling_avg_3d
from tweets order by 1,2 */

select 
t1.user_id,
t1.tweet_date,
(select round(Avg(t2.tweet_count),2)
from tweets  t2 
where t2.user_id=t1.user_id and t2.tweet_date between t1.tweet_date- 
interval '2 days' and t1.tweet_date) as rolling_avg_3d
from tweets t1;
