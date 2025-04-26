# Credit Card Issuance Disparity Analysis

## Problem Statement

Your team at JPMorgan Chase is preparing to launch a new credit card, and to gain some insights, you're analyzing how many credit cards were issued each month.

**Goal**: Write a query that outputs the name of each credit card and the difference in the number of issued cards between the month with the highest issuance and the lowest issuance. Arrange the results based on the largest disparity.

---

## Table: `monthly_cards_issued`

| Column Name     | Type     |
|-----------------|----------|
| card_name       | string   |
| issued_amount   | integer  |
| issue_month     | integer  |
| issue_year      | integer  |

---

## Example Input

| card_name             | issued_amount | issue_month | issue_year |
|-----------------------|----------------|-------------|-------------|
| Chase Freedom Flex    | 55000          | 1           | 2021        |
| Chase Freedom Flex    | 60000          | 2           | 2021        |
| Chase Freedom Flex    | 65000          | 3           | 2021        |
| Chase Freedom Flex    | 70000          | 4           | 2021        |
| Chase Sapphire Reserve| 170000         | 1           | 2021        |
| Chase Sapphire Reserve| 175000         | 2           | 2021        |
| Chase Sapphire Reserve| 180000         | 3           | 2021        |

---

## Example Output

| card_name              | difference |
|------------------------|------------|
| Chase Freedom Flex     | 15000      |
| Chase Sapphire Reserve | 10000      |

> Chase Freedom Flex's best month was 70k cards issued and the worst month was 55k cards, so the difference is 15k cards.  
> Chase Sapphire Reserve’s best month was 180k cards issued and the worst month was 170k cards, so the difference is 10k cards.

---

## SQL Query

```sql
with cte as (SELECT card_name,max(issued_amount) as max_issued_amount,
min(issued_amount) as min_issued_amount FROM monthly_cards_issued 
group by 1)

select card_name,(max_issued_amount-min_issued_amount) as difference from cte order by 2 desc  ;





