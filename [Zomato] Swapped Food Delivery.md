# 🛠️ Zomato Order Correction Problem

## 📘 Problem Description

Zomato is a leading online food delivery service that connects users with various restaurants and cuisines, allowing them to browse menus, place orders, and get meals delivered to their doorsteps.

Recently, Zomato encountered an issue with their delivery system. Due to an error in the delivery driver instructions, each item's order was **swapped with the item in the subsequent row**.

As a **data analyst**, you're asked to **correct this swapping error** and return the proper pairing of `order_id` and `item`.

---

## 📋 Requirements

- Correct the order-item pairing based on the swapping error.
- If the **last order ID is odd**, it should remain unchanged in the corrected result.

---

## 🧾 Input Schema

### `orders` Table

| column_name | type    | description                      |
|-------------|---------|----------------------------------|
| order_id    | integer | The ID of each Zomato order.     |
| item        | string  | The name of the food item ordered |

---

## 🧪 Sample Input

| order_id | item            |
|----------|-----------------|
| 1        | Chow Mein       |
| 2        | Pizza           |
| 3        | Pad Thai        |
| 4        | Butter Chicken  |
| 5        | Eggrolls        |
| 6        | Burger          |
| 7        | Tandoori Chicken|

---

## ✅ Expected Output

| corrected_order_id | item             |
|--------------------|------------------|
| 1                  | Pizza            |
| 2                  | Chow Mein        |
| 3                  | Butter Chicken   |
| 4                  | Pad Thai         |
| 5                  | Burger           |
| 6                  | Eggrolls         |
| 7                  | Tandoori Chicken |

- Order ID 1 is now associated with Pizza.
- Order ID 2 is paired with Chow Mein.
- Order ID 7 (an odd number) remains unchanged.

---

## 💡 Notes

- The correction ensures that every even-numbered item is swapped with its previous odd-numbered counterpart.
- This problem assumes that the error happened for **every adjacent pair**.
- For **odd-length datasets**, the **last item** (if `order_id` is odd) should **remain unchanged**.

---

## 💻 SQL Solution Idea

A possible approach using SQL window functions:

```sql
select case when order_id=(select max(order_id) from orders) then order_id
when order_id % 2 = 0 then order_id-1 
else order_id+1 end 
as corrected_order_id, item from orders  order by 1 ;
