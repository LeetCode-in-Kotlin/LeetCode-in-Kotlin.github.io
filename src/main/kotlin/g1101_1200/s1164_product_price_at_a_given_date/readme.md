[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1164\. Product Price at a Given Date

Medium

SQL Schema

Table: `Products`

    +---------------+---------+ 
    | Column Name   | Type    | 
    +---------------+---------+ 
    | product_id    | int     | 
    | new_price     | int     | 
    | change_date   | date    | 
    +---------------+---------+ 
    
(product_id, change_date) is the primary key of this table. 

Each row of this table indicates that the price of some product was changed to a new price at some date.

Write an SQL query to find the prices of all products on `2019-08-16`. Assume the price of all products before any change is `10`.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:** Products table: 

    +------------+-----------+-------------+ 
    | product_id | new_price | change_date | 
    +------------+-----------+-------------+ 
    | 1          | 20        | 2019-08-14  | 
    | 2          | 50        | 2019-08-14  | 
    | 1          | 30        | 2019-08-15  | 
    | 1          | 35        | 2019-08-16  | 
    | 2          | 65        | 2019-08-17  |
    | 3          | 20        | 2019-08-18  | 
    +------------+-----------+-------------+

**Output:** 

    +------------+-------+ 
    | product_id | price | 
    +------------+-------+ 
    | 2          | 50    | 
    | 1          | 35    | 
    | 3          | 10    | 
    +------------+-------+

## Solution

```sql
# Write your MySQL query statement below
WITH before_change_date AS (
    SELECT DISTINCT
        product_id,
        new_price as price,
        RANK() Over(Partition By product_id Order by change_date DESC) as rnk
    FROM Products
    WHERE change_date <= '2019-08-16'
)

SELECT
    product_id,
    price
FROM before_change_date
WHERE rnk = 1

UNION

SELECT
  DISTINCT product_id,
  10 as price
FROM Products
WHERE product_id not in (
      SELECT  product_id
      FROM Products
      WHERE change_date <= '2019-08-16'
)
```