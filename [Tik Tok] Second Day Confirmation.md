# TikTok Second-Day Confirmation Query

## Problem Statement

Given tables `emails` and `texts`, identify the users who did not confirm their TikTok sign-up on the same day they signed up but confirmed it the following day. Each user signs up using their email address and receives a text message to confirm their account.

### Table Definitions

**emails**
| Column Name | Type     |
|-------------|----------|
| email_id    | integer  |
| user_id     | integer  |
| signup_date | datetime |


### Sample Data:

| email_id | user_id | signup_date        |
|----------|---------|--------------------|
| 125      | 7771    | 2022-06-14 00:00:00 |
| 433      | 1052    | 2022-07-09 00:00:00 |

---

### 2. `texts` Table

| Column Name   | Type     | Description                                |
|---------------|----------|--------------------------------------------|
| text_id       | integer  | Unique ID for each text confirmation       |
| email_id      | integer  | Foreign key referencing `emails.email_id`  |
| signup_action | string   | Status of confirmation ('Confirmed' / 'Not Confirmed') |
| action_date   | datetime | Date and time when the action was taken    |

### Sample Data:

| text_id | email_id | signup_action | action_date          |
|---------|----------|----------------|-----------------------|
| 6878    | 125      | Confirmed      | 2022-06-14 00:00:00  |
| 6997    | 433      | Not Confirmed  | 2022-07-09 00:00:00  |
| 7000    | 433      | Confirmed      | 2022-07-10 00:00:00  |


### Objective

Find the `user_id`s of users who:
- **Did not confirm** their sign-up on the **signup_date**.
- **Did confirm** on the **next day** (i.e., signup_date + 1).

## SQL Query

```sql
SELECT a.user_id FROM emails a inner join texts b 
on a.email_id=b.email_id where 
Extract(day from(cast(b.action_date as timestamp)-
cast(a.signup_date as date)))=1;
