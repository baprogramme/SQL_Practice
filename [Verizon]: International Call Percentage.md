### Problem: Percentage of International Phone Calls

A phone call is considered an **international call** when the person calling is in a different country than the person receiving the call.

#### Task:
What percentage of phone calls are international? **Round the result to 1 decimal**.

#### Assumption:
- The `caller_id` in the `phone_info` table refers to both the caller and receiver.

---

### Tables

#### `phone_calls` Table

| Column Name  | Type      |
|--------------|-----------|
| caller_id    | integer   |
| receiver_id  | integer   |
| call_time    | timestamp |

##### Example Input:

| caller_id | receiver_id | call_time           |
|-----------|-------------|---------------------|
| 1         | 2           | 2022-07-04 10:13:49 |
| 1         | 5           | 2022-08-21 23:54:56 |
| 5         | 1           | 2022-05-13 17:24:06 |
| 5         | 6           | 2022-03-18 12:11:49 |

---

#### `phone_info` Table

| Column Name  | Type      |
|--------------|-----------|
| caller_id    | integer   |
| country_id   | integer   |
| network      | integer   |
| phone_number | string    |

##### Example Input:

| caller_id | country_id | network  | phone_number     |
|-----------|------------|----------|------------------|
| 1         | US         | Verizon  | +1-212-897-1964  |
| 2         | US         | Verizon  | +1-703-346-9529  |
| 3         | US         | Verizon  | +1-650-828-4774  |
| 4         | US         | Verizon  | +1-415-224-6663  |
| 5         | IN         | Vodafone | +91 7503-907302  |
| 6         | IN         | Vodafone | +91 2287-664895  |

---

### Example Output

| international_calls_pct |
|-------------------------|
| 50.0                    |

---

### Explanation

There is a total of 4 calls with 2 of them being international:
- From caller_id `1` to receiver_id `5`
- From caller_id `5` to receiver_id `1`

Thus, **2 / 4 = 50.0%**

> The dataset you are querying against may have different input & output - this is just an example!

``` sql

with cte1 as (
select a.caller_id as caller_id,b.country_id as call_country, a.receiver_id as receiver_id from ((select * from phone_calls  order by caller_id) a left join phone_info b on a.caller_id=b.caller_id) 
order by a.caller_id ), cte2 as (

select a.caller_id as caller_id,b.country_id as receive_country, a.receiver_id as receiver_id from ((select * from phone_calls  order by caller_id) a left join phone_info b on a.receiver_id=b.caller_id) 
order by a.caller_id ), cte3 as (

select cte1.caller_id,cte1.call_country as calling_country, cte2.receiver_id,cte2.receive_country as receiving_country from cte1 inner join cte2 on 
cte1.caller_id=cte2.caller_id and cte1.receiver_id=cte2.receiver_id )

select round((100*(1.0*sum(case when calling_country<>receiving_country then 1 else 0 end))/(count(caller_id)*1.0)),1) as international_calls_pct from 
cte3
;
