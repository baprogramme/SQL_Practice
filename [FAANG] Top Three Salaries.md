
# High Earners by Department SQL Report

## Problem Statement

As part of an ongoing analysis of salary distribution within the company, your manager has requested a report identifying **high earners in each department**.

A **'high earner'** within a department is defined as an employee with a salary ranking among the **top three salaries within that department**.

## Objective

Identify these high earners across all departments and display:

- **Employee's Name**
- **Department Name**
- **Salary**

### Sorting Requirements

- Results should be sorted **first by department name (ascending)**,
- Then by **salary (descending)**,
- If multiple employees have the same salary, order them **alphabetically by name**.

## Notes

- Use appropriate **ranking window functions** to manage duplicate salaries effectively.
- Ensure that **duplicate salaries are allowed** within the top three (no uniqueness required).

## Schema

### `employee` Table

| Column Name   | Type    | Description                          |
|---------------|---------|--------------------------------------|
| employee_id   | integer | The unique ID of the employee        |
| name          | string  | The name of the employee             |
| salary        | integer | The salary of the employee           |
| department_id | integer | The department ID of the employee    |
| manager_id    | integer | The manager ID of the employee       |

### `department` Table

| Column Name     | Type    | Description                       |
|------------------|---------|-----------------------------------|
| department_id    | integer | The department ID of the employee|
| department_name  | string  | The name of the department        |

## Example Input

### `employee` Table

| employee_id | name             | salary | department_id | manager_id |
|-------------|------------------|--------|---------------|------------|
| 1           | Emma Thompson    | 3800   | 1             | 6          |
| 2           | Daniel Rodriguez | 2230   | 1             | 7          |
| 3           | Olivia Smith     | 2000   | 1             | 8          |
| 4           | Noah Johnson     | 6800   | 2             | 9          |
| 5           | Sophia Martinez  | 1750   | 1             | 11         |
| 6           | Liam Brown       | 13000  | 3             |            |
| 7           | Ava Garcia       | 12500  | 3             |            |
| 8           | William Davis    | 6800   | 2             |            |
| 9           | Isabella Wilson  | 11000  | 3             |            |
| 10          | James Anderson   | 4000   | 1             | 11         |

### `department` Table

| department_id | department_name |
|---------------|-----------------|
| 1             | Data Analytics  |
| 2             | Data Science    |

## Example Output

| department_name | name             | salary |
|------------------|------------------|--------|
| Data Analytics   | James Anderson   | 4000   |
| Data Analytics   | Emma Thompson    | 3800   |
| Data Analytics   | Daniel Rodriguez | 2230   |
| Data Science     | Noah Johnson     | 6800   |
| Data Science     | William Davis    | 6800   |

## Explanation

- In **Data Analytics**, the top three earners are: James Anderson (4000), Emma Thompson (3800), and Daniel Rodriguez (2230).
- In **Data Science**, Noah Johnson and William Davis both earn 6800. Since the result is ordered alphabetically, Noah appears first.

## SQL Solution (Optional)

```sql
SELECT
    d.department_name,
    e.name,
    e.salary
FROM (
    SELECT *,
           DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank_in_dept
    FROM employee
) e
JOIN department d ON e.department_id = d.department_id
WHERE rank_in_dept <= 3
ORDER BY d.department_name ASC, e.salary DESC, e.name ASC;
```

# OR
```sql
with cte as (SELECT a.name,a.salary,b.department_name,dense_rank() over (partition by 
b.department_name order by a.salary desc) as rank FROM employee a inner join department  b on 
a.department_id=b.department_id  )

select department_name,name,salary from cte where rank<=3 order by 1,3 desc,2;
```



