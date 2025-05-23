## Problem: Update Advertiser Payment Status

You're provided with two tables:

- **`advertiser`**: Contains information about advertisers and their respective payment status.
- **`daily_pay`**: Contains the current payment information for advertisers (only includes advertisers who made payments).

### Goal

Write a SQL query to **update the payment status** of Facebook advertisers based on the information in the `daily_pay` table. The result should include:

- `user_id`
- Their **current payment status** (`new_status`)
- Sorted by `user_id`.

---

### Payment Status Rules

Payment status of advertisers can be:

- `NEW`: Newly registered advertisers who have made their first payment.
- `EXISTING`: Advertisers who have made payments in the past and paid again recently.
- `CHURN`: Advertisers who have made payments in the past but not recently.
- `RESURRECT`: Advertisers who didn’t pay recently but have resumed payment after being inactive.

---

### Transition Table

| # | Current Status | Payment on Day T | Updated Status |
|---|----------------|------------------|----------------|
| 1 | NEW            | Paid             | EXISTING       |
| 2 | NEW            | Not paid         | CHURN          |
| 3 | EXISTING       | Paid             | EXISTING       |
| 4 | EXISTING       | Not paid         | CHURN          |
| 5 | CHURN          | Paid             | RESURRECT      |
| 6 | CHURN          | Not paid         | CHURN          |
| 7 | RESURRECT      | Paid             | EXISTING       |
| 8 | RESURRECT      | Not paid         | CHURN          |

**Summary:**
- If no payment is made on day T → status becomes `CHURN`.
- If payment **is** made:
  - Previous status `CHURN` → `RESURRECT`
  - All others → `EXISTING`

---

### Tables

#### advertiser Table

| Column Name | Type   |
|-------------|--------|
| user_id     | string |
| status      | string |

**Example:**

| user_id | status   |
|---------|----------|
| bing    | NEW      |
| yahoo   | NEW      |
| alibaba | EXISTING |

---

#### daily_pay Table

| Column Name | Type   |
|-------------|--------|
| user_id     | string |
| paid        | decimal |

**Example:**

| user_id | paid  |
|---------|-------|
| yahoo   | 45.00 |
| alibaba | 100.00 |
| target  | 13.00  |

---

### Expected Output

| user_id | new_status |
|---------|------------|
| alibaba | EXISTING   |
| bing    | CHURN      |
| yahoo   | EXISTING   |

---

### Explanation

- `bing`: No payment record → becomes `CHURN`
- `yahoo`: Paid → goes from `NEW` to `EXISTING`
- `alibaba`: Paid again → stays `EXISTING`

Note: The dataset you are querying against may have different inputs and outputs — the above is just an example!

``` sql

with cte1 as (SELECT coalesce(a.user_id,b.user_id) as user_id,a.paid,b.status FROM daily_pay  a 
full outer join advertiser b on a.user_id=b.user_id ) 

select user_id, case 
when status is null then 'NEW'
when status= 'NEW' and paid is null then 'CHURN' 
when status= 'NEW' and paid is not null then 'EXISTING' 
when status= 'EXISTING' and paid is not null then 'EXISTING' 
when status= 'EXISTING' and paid is null then 'CHURN' 
when status= 'CHURN' and paid is not null then 'RESURRECT' 
when status= 'CHURN' and paid is null then 'CHURN' 
when status= 'RESURRECT' and paid is not null then 'EXISTING' 
when status= 'RESURRECT' and paid is null then 'CHURN' End  as new_status
from cte1 order by user_id;
