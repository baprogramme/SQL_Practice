# Top 3 Cities by Completed Trade Orders

## Problem Statement

Assume you're given the tables containing completed trade orders and user details in a Robinhood trading system.

Write a SQL query to retrieve the **top three cities** that have the **highest number of completed trade orders**, listed in **descending order**.

### Output:
- **city**: Name of the city
- **total_orders**: Number of completed trade orders from users in that city

---

## Table Schemas

### `trades` Table

| Column Name | Type     | Description                              |
|-------------|----------|------------------------------------------|
| order_id    | integer  | Unique identifier for the trade order    |
| user_id     | integer  | Foreign key to `users.user_id`           |
| quantity    | integer  | Quantity of assets traded                |
| status      | string   | Trade status: 'Completed' or 'Cancelled' |
| date        | datetime | Date and time of the trade               |
| price       | decimal  | Trade price per asset                    |

### `users` Table

| Column Name | Type     | Description                              |
|-------------|----------|------------------------------------------|
| user_id     | integer  | Unique identifier for the user           |
| city        | string   | City where the user resides              |
| email       | string   | Email address of the user                |
| signup_date | datetime | Date when the user signed up             |

---

## Example Input

### `trades` Table

| order_id | user_id | quantity | status    | date                | price |
|----------|---------|----------|-----------|---------------------|--------|
| 100101   | 111     | 10       | Cancelled | 08/17/2022 12:00:00 | 9.80   |
| 100102   | 111     | 10       | Completed | 08/17/2022 12:00:00 | 10.00  |
| 100259   | 148     | 35       | Completed | 08/25/2022 12:00:00 | 5.10   |
| 100264   | 148     | 40       | Completed | 08/26/2022 12:00:00 | 4.80   |
| 100305   | 300     | 15       | Completed | 09/05/2022 12:00:00 | 10.00  |
| 100400   | 178     | 32       | Completed | 09/17/2022 12:00:00 | 12.00  |
| 100565   | 265     | 2        | Completed | 09/27/2022 12:00:00 | 8.70   |

### `users` Table

| user_id | city          | email                          | signup_date         |
|---------|---------------|--------------------------------|---------------------|
| 111     | San Francisco | rrok10@gmail.com               | 08/03/2021 12:00:00 |
| 148     | Boston        | sailor9820@gmail.com           | 08/20/2021 12:00:00 |
| 178     | San Francisco | harrypotterfan182@gmail.com    | 01/05/2022 12:00:00 |
| 265     | Denver        | shadower_@hotmail.com          | 02/26/2022 12:00:00 |
| 300     | San Francisco | houstoncowboy1122@hotmail.com  | 06/30/2022 12:00:00 |

---

## Example Output

| city          | total_orders |
|---------------|---------------|
| San Francisco | 3             |
| Boston        | 2             |
| Denver        | 1             |

---

## SQL Query

```sql
SELECT b.city as city, count(distinct a.order_id) as total_orders FROM trades a inner join users b on a.user_id=b.user_id 
where a.status= 'Completed' group by 1 order by 2 desc limit 3 ;
