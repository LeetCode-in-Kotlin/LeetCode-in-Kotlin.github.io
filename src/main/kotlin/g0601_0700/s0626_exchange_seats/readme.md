[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 626\. Exchange Seats

Medium

SQL Schema

Table: `Seat`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | id          | int     |
    | name        | varchar |
    +-------------+---------+
    id is the primary key column for this table.
    Each row of this table indicates the name and the ID of a student.
    id is a continuous increment. 

Write an SQL query to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by `id` **in ascending order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Seat table:
    +----+---------+
    | id | student |
    +----+---------+
    | 1  | Abbot   |
    | 2  | Doris   |
    | 3  | Emerson |
    | 4  | Green   |
    | 5  | Jeames  |
    +----+---------+

**Output:**

    +----+---------+
    | id | student |
    +----+---------+
    | 1  | Doris   |
    | 2  | Abbot   |
    | 3  | Green   |
    | 4  | Emerson |
    | 5  | Jeames  |
    +----+---------+

**Explanation:**

Note that if the number of students is odd, there is no need to change the last one's seat.

## Solution

```sql
# Write your MySQL query statement below
SELECT CASE
    WHEN s.id = ( SELECT COUNT(*) FROM Seat ) AND MOD(s.id,2) = 1 THEN s.id
    WHEN MOD(s.id,2) = 0 THEN s.id - 1
    ELSE s.id + 1
    END AS id,
    s.student
FROM Seat AS s
ORDER BY id
```