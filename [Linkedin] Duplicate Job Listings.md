
# Duplicate Job Listings Query

This project contains a SQL query to identify companies that have posted **duplicate job listings** on the LinkedIn platform.

## 📌 Problem Statement

You're given a table `job_listings` containing job postings from various companies. Your task is to retrieve the **count of companies** that have posted **duplicate job listings**.

### 🔁 Definition of Duplicate
Duplicate job listings are defined as **two or more job listings within the same company** that have **identical titles and descriptions**.

## 🗃️ Table Schema

**Table Name:** `job_listings`

| Column Name | Type     | Description                    |
|-------------|----------|--------------------------------|
| job_id      | integer  | Unique identifier for the job |
| company_id  | integer  | Unique identifier for the company |
| title       | string   | Job title                      |
| description | string   | Job description                |

## 📥 Example Input

| job_id | company_id | title           | description                                                                                                                       |
|--------|------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| 248    | 827        | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations. |
| 149    | 845        | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations. |
| 945    | 345        | Data Analyst     | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.          |
| 164    | 345        | Data Analyst     | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems.          |
| 172    | 244        | Data Engineer    | Data engineer works in a variety of settings to build systems that collect, manage, and convert raw data into usable information for data scientists and business analysts to interpret. |

## 📤 Example Output

| duplicate_companies |
|---------------------|
| 1                   |

**Explanation:** Company `345` posted duplicate job listings with the same title and description.

## 🧠 SQL Query

```sql
select count(*) from (select company_id,title,description
from job_listings group by 1,2,3 having count(*)>1)as temp;
