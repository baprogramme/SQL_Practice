# Employee vs Manager Salary Analysis

## Overview

This project is designed to analyze employee salary data to identify cases where employees earn **more than their direct managers**. This type of analysis helps companies ensure fair and structured compensation practices.

As a Human Resources (HR) Analyst, the goal is to extract and present a list of employees who surpass their managers in salary.

---

## Problem Statement

**Task**:  
Identify all employees who earn **more** than their **direct managers**.

**Expected Output**:  
The result should include the employee's ID and name.

---

## Dataset Schema

The analysis is based on the following table:

### `employee` Table

| Column Name     | Type     | Description                            |
|-----------------|----------|----------------------------------------|
| employee_id     | integer  | The unique ID of the employee.         |
| name            | string   | The name of the employee.              |
| salary          | integer  | The salary of the employee.            |
| department_id   | integer  | The department ID of the employee.     |
| manager_id      | integer  | The manager ID of the employee.        |

---

## Example Input

| employee_id | name             | salary | department_id | manager_id |
|-------------|------------------|--------|----------------|-------------|
| 1           | Emma Thompson    | 3800   | 1              | 6           |
| 2           | Daniel Rodriguez | 2230   | 1              | 7           |
| 3           | Olivia Smith     | 7000   | 1              | 8           |
| 4           | Noah Johnson     | 6800   | 2              | 9           |
| 5           | Sophia Martinez  | 1750   | 1              | 11          |
| 6           | Liam Brown       | 13000  | 3              | NULL        |
| 7           | Ava Garcia       | 12500  | 3              | NULL        |
| 8           | William Davis    | 6800   | 2              | NULL        |

---

## Example Output

| employee_id | employee_name  |
|-------------|----------------|
| 3           | Olivia Smith   |

**Explanation**:  
Olivia Smith earns $7,000 while her manager, William Davis, earns $6,800.

---

## SQL Query (Example)

```sql
SELECT a.employee_id,a.name FROM employee a
inner join (select employee_id,salary from employee) b on a.manager_id=b.employee_id where a.salary>b.salary;
