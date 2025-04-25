# IBM Db2 Query Utilization Histogram

## Problem Statement

IBM is analyzing how their employees are utilizing the Db2 database by tracking the SQL queries executed by their employees. The objective is to generate data to populate a histogram that shows the number of **unique queries** run by employees during the **third quarter of 2023 (July to September)**. Additionally, it should count the number of employees who **did not run any queries** during this period.

The final output should display:
- `unique_queries`: The number of unique queries run by an employee during Q3 2023.
- `employee_count`: The number of employees who executed that number of unique queries.

## Table Schemas

### `queries` Table

| Column Name     | Type      | Description                                      |
|------------------|-----------|--------------------------------------------------|
| employee_id      | integer   | The ID of the employee who executed the query.   |
| query_id         | integer   | The unique identifier for each query (Primary Key). |
| query_starttime  | datetime  | The timestamp when the query started.            |
| execution_time   | integer   | The duration of the query execution in seconds.  |

#### Example Input:

| employee_id | query_id | query_starttime     | execution_time |
|-------------|----------|---------------------|----------------|
| 226         | 856987   | 07/01/2023 01:04:43  | 2698           |
| 132         | 286115   | 07/01/2023 03:25:12  | 2705           |
| 221         | 33683    | 07/01/2023 04:34:38  | 91             |
| 240         | 17745    | 07/01/2023 14:33:47  | 2093           |
| 110         | 413477   | 07/02/2023 10:55:14  | 470            |

---

### `employees` Table

| Column Name  | Type    | Description                             |
|--------------|---------|-----------------------------------------|
| employee_id  | integer | The ID of the employee.                 |
| full_name    | string  | The full name of the employee.          |
| gender       | string  | The gender of the employee.             |

#### Example Input:

| employee_id | full_name         | gender |
|-------------|-------------------|--------|
| 1           | Judas Beardon     | Male   |
| 2           | Lainey Franciotti | Female |
| 3           | Ashbey Strahan    | Male   |

---

## Expected Output Format

| unique_queries | employee_count |
|----------------|----------------|
| 0              | 191            |
| 1              | 46             |
| 2              | 12             |
| 3              | 1              |

- This indicates:
  - 191 employees did not run any queries.
  - 46 employees ran exactly 1 unique query.
  - 12 employees ran exactly 2 unique queries.
  - 1 employee ran exactly 3 unique queries.

>  Note: The dataset you are querying against may have different input and output values — this is just an example.

## SQL Query

```sql
with cte as (SELECT a.employee_id as employee,count(distinct b.query_id) as unique_queries FROM 
employees a left join queries b on a.employee_id=b.employee_id and 
extract(month from cast(query_starttime as timestamp))>=7 and
extract(month from cast(query_starttime as timestamp))<=9
and execution_time is not null
group by 1)

select unique_queries,count(employee) from cte group by 1 order by 1
;
