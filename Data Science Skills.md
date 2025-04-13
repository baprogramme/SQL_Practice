# 📊 Find Qualified Data Science Candidates

## 📝 Problem Statement

Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in **Python**, **Tableau**, and **PostgreSQL**.

### Assumptions:
- There are **no duplicate rows** in the `candidates` table.
- Each row represents one skill for a candidate.

---

## 📂 Table Structure

### `candidates` Table

| Column Name   | Type     |
|---------------|----------|
| candidate_id  | integer  |
| skill         | varchar  |

---

## 📥 Example Input

| candidate_id | skill       |
|--------------|-------------|
| 123          | Python      |
| 123          | Tableau     |
| 123          | PostgreSQL  |
| 234          | R           |
| 234          | PowerBI     |
| 234          | SQL Server  |
| 345          | Python      |
| 345          | Tableau     |

---

## ✅ Expected Output

| candidate_id |
|--------------|
| 123          |

### 🎯 Explanation

Candidate `123` is selected because they have all three required skills: Python, Tableau, and PostgreSQL.  
Candidate `345` is not selected because they're missing PostgreSQL.

---

## 💡 SQL Query

```sql
select candidate_id from (Select candidate_id,count(distinct skill) as number_of_skill from (SELECT candidate_id,
skill FROM candidates where upper(skill) in ('PYTHON',
'TABLEAU','POSTGRESQL')) AS Candidate_with_skill group by 1) as Candidate_with_all_skill order by number_of_skill desc limit 1;
