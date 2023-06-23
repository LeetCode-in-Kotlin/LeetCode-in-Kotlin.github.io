[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1907\. Count Salary Categories

Medium

SQL Schema

Table: `Accounts`

    +-------------+------+
    | Column Name | Type |
    +-------------+------+
    | account_id  | int  |
    | income      | int  |
    +-------------+------+
    account_id is the primary key for this table.
    Each row contains information about the monthly income for one bank account. 

Write an SQL query to report the number of bank accounts of each salary category. The salary categories are:

*   `"Low Salary"`: All the salaries **strictly less** than `$20000`.
*   `"Average Salary"`: All the salaries in the **inclusive** range `[$20000, $50000]`.
*   `"High Salary"`: All the salaries **strictly greater** than `$50000`.

The result table **must** contain all three categories. If there are no accounts in a category, then report `0`.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Accounts table:
    +------------+--------+
    | account_id | income |
    +------------+--------+
    | 3          | 108939 |
    | 2          | 12747  |
    | 8          | 87709  |
    | 6          | 91796  |
    +------------+--------+

**Output:**

    +----------------+----------------+
    | category       | accounts_count |
    +----------------+----------------+
    | Low Salary     | 1              |
    | Average Salary | 0              |
    | High Salary    | 3              |
    +----------------+----------------+

**Explanation:**

Low Salary: Account 2.

Average Salary: No accounts.

High Salary: Accounts 3, 6, and 8.

## Solution

```sql
# Write your MySQL query statement below
with cte1 as (SELECT 'Low Salary' AS category,SUM(CASE WHEN INCOME<20000 THEN 1 ELSE 0 END ) AS accounts_count from Accounts),
cte2 as(SELECT 'Average Salary' AS category,SUM(CASE WHEN INCOME BETWEEN 20000 AND 50000 THEN 1 ELSE 0 END ) AS accounts_count from Accounts),
cte3 as (SELECT 'High Salary' AS category,SUM(CASE WHEN INCOME> 50000 THEN  1 ELSE 0 END) AS accounts_count
FROM Accounts)
SELECT * from cte3
UNION ALL
SELECT * FROM cte1
UNION ALL
SELECT * FROM cte2
```