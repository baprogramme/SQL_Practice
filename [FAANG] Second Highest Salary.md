# Second Highest Salary Analysis

## 📄 Problem Description

You are an HR analyst at a tech company tasked with analyzing employee salaries.  
Your manager wants to understand the pay distribution and has asked you to determine **the second highest salary among all employees**.

👉 **Important points:**
- Multiple employees may share the same second highest salary.
- In case of duplicates, display the salary only once.

---

## 🗄️ Database Schema

**employee Table**

| column_name   | type     | description                         |
|---------------|----------|-------------------------------------|
| employee_id   | integer  | The unique ID of the employee.      |
| name          | string   | The name of the employee.           |
| salary        | integer  | The salary of the employee.        |
| department_id | integer  | The department ID of the employee. |
| manager_id    | integer  | The manager ID of the employee.    |

---

## 📥 Example Input

| employee_id | name             | salary | department_id | manager_id |
|-------------|------------------|--------|---------------|------------|
| 1          | Emma Thompson     | 3800   | 1             | 6          |
| 2          | Daniel Rodriguez  | 2230   | 1             | 7          |
| 3          | Olivia Smith      | 2000   | 1             | 8          |

---

## 📤 Example Output

| second_highest_salary |
|------------------------|
| 2230                 |

> **Explanation:**  
> The highest salary is `3800`, the second highest is `2230`.  
> Even if multiple employees had a salary of `2230`, it would appear only once.

---

## 🛠️ SQL Query

```sql
with cte as (SELECT *, ROW_NUMBER() over(order by salary desc)
as rank
FROM employee )

select salary as second_highest_salary from cte where rank=2
;
