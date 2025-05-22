### 🧠 Problem: Median Number of Searches per User

**Context:**  
Google's marketing team is making a Superbowl commercial and needs a simple statistic to put on their TV ad: the median number of searches a person made last year.

However, at Google scale, querying the 2 trillion searches is too costly. Luckily, you have access to a **summary table** which tells you:
- The number of searches made last year (`searches`)
- How many Google users made that many searches (`num_users`)

Your task is to **write a SQL query** to report the **median** of searches made by a user.

---

### 🔢 Table: `search_frequency`

| Column Name  | Type     |
|--------------|----------|
| searches     | integer  |
| num_users    | integer  |

---

### 📥 Example Input:

| searches | num_users |
|----------|-----------|
| 1        | 2         |
| 2        | 2         |
| 3        | 3         |
| 4        | 1         |

---

### 📤 Example Output:

| median |
|--------|
| 2.5    |

---

### 🧮 Explanation:

By expanding the `search_frequency` table, we get this distribution of searches per user:

``` sql

with cte as (
SELECT *,SUM(num_users) OVER(ORDER BY searches) as running_sum 
, sum(num_users) over () usersum
FROM search_frequency )


select round(avg(searches),1) from  cte  
where running_sum in (floor((usersum+1)*1.0/2) , ceiling((usersum+1)*1.0/2))
