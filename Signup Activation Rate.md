
# TikTok User Activation Rate Analysis

## Problem Statement

New TikTok users sign up with their emails. They confirm their signup by replying to a text confirmation to activate their accounts. Users may receive multiple text messages for account confirmation until they have confirmed their new account.

A senior analyst is interested to know the activation rate of specified users in the `emails` table. Write a query to find the activation rate. Round the percentage to 2 decimal places.

---

## Definitions

- **`emails` table**: Contains the information of user signup details.
- **`texts` table**: Contains the users' activation information.

---

## Assumptions

- The analyst is interested in the activation rate of specific users in the `emails` table, which may not include all users that could potentially be found in the `texts` table.
- For example, user 123 in the `emails` table may not be in the `texts` table and vice versa.
- Effective April 4th 2023, we added an assumption to the question to provide additional clarity.

---

## Schema

### emails Table

| Column Name | Type     |
|-------------|----------|
| email_id    | integer  |
| user_id     | integer  |
| signup_date | datetime |

### texts Table

| Column Name   | Type     |
|---------------|----------|
| text_id       | integer  |
| email_id      | integer  |
| signup_action | varchar  |

---

## Sample Input

**emails Table**

| email_id | user_id | signup_date         |
|----------|---------|---------------------|
| 125      | 7771    | 06/14/2022 00:00:00 |
| 236      | 6950    | 07/01/2022 00:00:00 |
| 433      | 1052    | 07/09/2022 00:00:00 |

**texts Table**

| text_id | email_id | signup_action |
|---------|----------|----------------|
| 6878    | 125      | Confirmed      |
| 6920    | 236      | Not Confirmed  |
| 6994    | 236      | Confirmed      |

---

## Expected Output

| confirm_rate |
|--------------|
| 0.67         |

### Explanation

67% of users in the `emails` table have successfully completed their signup and activated their accounts. The remaining 33% have not yet replied to the text to confirm their signup.

Note: The dataset you are querying against may have different input & output - this is just an example.


``` sql

select round(1.0*sum(case when b.signup_action='Confirmed' 
then 1 else 0 end)/count(distinct a.email_id),2) from emails a left join texts b on a.email_id=b.email_id

