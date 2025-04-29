# CVS Health Pharmacy Sales Loss Analysis

CVS Health is analyzing its pharmacy sales data, and how well different products are selling in the market. Each drug is exclusively manufactured by a single manufacturer.

Write a query to identify the manufacturers associated with the drugs that resulted in losses for CVS Health and calculate the total amount of losses incurred.

**Output**:  
- Manufacturer's name
- Number of drugs associated with losses
- Total losses in absolute value

**Sort** the results in descending order by the highest losses.

---

## Table: pharmacy_sales

| Column Name   | Type     |
|---------------|----------|
| product_id    | integer  |
| units_sold    | integer  |
| total_sales   | decimal  |
| cogs          | decimal  |
| manufacturer  | varchar  |
| drug          | varchar  |

---

## Example Input

| product_id | units_sold | total_sales | cogs       | manufacturer | drug                      |
|------------|------------|-------------|------------|--------------|---------------------------|
| 156        | 89514      | 3130097.00  | 3427421.73 | Biogen       | Acyclovir                 |
| 25         | 222331     | 2753546.00  | 2974975.36 | AbbVie       | Lamivudine and Zidovudine |
| 50         | 90484      | 2521023.73  | 2742445.90 | Eli Lilly    | Dermasorb TA Complete Kit |
| 98         | 110746     | 813188.82   | 140422.87  | Biogen       | Medi-Chord                |

---

## Example Output

| manufacturer | drug_count | total_loss |
|--------------|------------|------------|
| Biogen       | 1          | 297324.73  |
| AbbVie       | 1          | 221429.36  |
| Eli Lilly    | 1          | 221422.17  |

---

## Explanation

- The first three rows indicate that some drugs resulted in losses.
- Among these, **Biogen** had the highest losses, followed by **AbbVie** and **Eli Lilly**.
- However, the *Medi-Chord* drug manufactured by Biogen reported a **profit** and was excluded from the result.

---

## SQL Query

```sql

/*with cte as (SELECT manufacturer,product_id as drug_count,
(cogs-total_sales) as total_loss 
 FROM pharmacy_sales group by 1)
 
 select manufacturer,drug_count,(cogs-total_sales) as total_loss from cte 
 order by 2 desc,3 desc */

SELECT manufacturer,COUNT(drug),SUM(cogs-total_sales) AS total_loss 
FROM pharmacy_sales
WHERE cogs>total_sales
GROUP BY manufacturer
ORDER BY total_loss DESC;
