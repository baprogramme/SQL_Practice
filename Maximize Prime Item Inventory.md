## Problem Statement

Amazon wants to maximize the storage capacity of its 500,000 square-foot warehouse by prioritizing a specific batch of **prime items**. The specific **prime product batch** detailed in the `item_category` column must be maintained.

So, if the prime product batch specified in the `item_category` column included **1 laptop** and **1 side table**, that would be the base batch. We could not add another laptop without also adding a side table; they come all together as a **batch set**.

### Constraints:

- Prioritize stocking **prime batches**
- After accommodating prime items, allocate any remaining space to **non-prime batches**
- Output the `item_type` with `prime_eligible` first followed by `not_prime`, along with the **maximum number of batches** that can be stocked

### Assumptions:

- Products must be stocked in batches, so we want to find the largest available quantity of **prime batches**, and then the largest available quantity of **non-prime batches**
- Non-prime items must always be available in stock to meet customer demand, so the **non-prime item count should never be zero**
- Item count should be **whole numbers (integers)**

---

## Table: `inventory`

| Column Name     | Type     |
|-----------------|----------|
| item_id         | integer  |
| item_type       | string   |
| item_category   | string   |
| square_footage  | decimal  |

---

## Example Input

| item_id | item_type       | item_category     | square_footage |
|---------|------------------|-------------------|----------------|
| 1374    | prime_eligible   | mini refrigerator | 68.00          |
| 4245    | not_prime        | standing lamp     | 26.40          |
| 2452    | prime_eligible   | television        | 85.00          |
| 3255    | not_prime        | side table        | 22.60          |
| 1672    | prime_eligible   | laptop            | 8.50           |

---

## Example Output

| item_type      | item_count |
|----------------|------------|
| prime_eligible | 9285       |
| not_prime      | 6          |

> **Note**: The dataset you are querying against may have different input & output - this is just an example!

``` sql


WITH CTE AS
(SELECT 
SUM(CASE WHEN ITEM_TYPE = 'not_prime' THEN 1 ELSE 0 END) N_PRIME,
SUM(CASE WHEN ITEM_TYPE = 'prime_eligible' THEN 1 ELSE 0 END) PRIME,
SUM(CASE WHEN ITEM_TYPE = 'not_prime' THEN SQUARE_FOOTAGE END) SUM_NP,
SUM(CASE WHEN ITEM_TYPE = 'prime_eligible' THEN SQUARE_FOOTAGE END) SUM_P
FROM INVENTORY)

SELECT 
'prime_eligible',
TRUNC(500000/SUM_P,0)*PRIME PRIME_ITEMS
FROM CTE
UNION ALL
SELECT 
'not_prime',
TRUNC((500000-TRUNC(500000/SUM_P,0)*SUM_P)/SUM_NP, 0)*N_PRIME NP_ITEMS
FROM CTE
