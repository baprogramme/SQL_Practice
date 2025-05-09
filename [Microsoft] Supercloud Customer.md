
# Microsoft Azure Supercloud Customer Identification

##  Problem Statement

A **Microsoft Azure Supercloud customer** is defined as a customer who has purchased at least **one product from every product category** listed in the `products` table.

Your task is to write a SQL query that identifies the **customer IDs** of these Supercloud customers.

---

### ˜ Tables and Example Data

#### `customer_contracts` Table:
| customer_id | product_id | amount |
|-------------|-------------|--------|
| 1           | 1           | 1000   |
| 1           | 3           | 2000   |
| 1           | 5           | 1500   |
| 2           | 2           | 3000   |
| 2           | 6           | 2000   |

#### `products` Table:
| product_id | product_category | product_name             |
|------------|------------------|--------------------------|
| 1          | Analytics         | Azure Databricks         |
| 2          | Analytics         | Azure Stream Analytics   |
| 4          | Containers        | Azure Kubernetes Service |
| 5          | Containers        | Azure Service Fabric     |
| 6          | Compute           | Virtual Machines         |
| 7          | Compute           | Azure Functions          |

---

### … Expected Output:
| customer_id |
|-------------|
| 1           |

---

### ¡ Explanation:
Customer `1` has purchased products from all categories: **Analytics**, **Containers**, and **Compute**  hence qualifies as a *Supercloud* customer.

Customer `2` did not purchase any products in the **Containers** category.

---

## … SQL Solution:

```sql
SELECT cc.customer_id
FROM customer_contracts cc
JOIN products p ON cc.product_id = p.product_id
GROUP BY cc.customer_id
HAVING COUNT(DISTINCT p.product_category) = (
    SELECT COUNT(DISTINCT product_category) FROM products
);
```

---

##   Key Concepts:
- Join data between `customer_contracts` and `products`.
- Count **distinct categories per customer**.
- Compare it with **total number of distinct product categories**.
