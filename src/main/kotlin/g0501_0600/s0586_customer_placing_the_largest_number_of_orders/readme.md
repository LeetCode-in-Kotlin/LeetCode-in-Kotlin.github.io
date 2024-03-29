[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 586\. Customer Placing the Largest Number of Orders

Easy

SQL Schema

Table: `Orders`

    +-----------------+----------+
    | Column Name     | Type     |
    +-----------------+----------+
    | order_number    | int      |
    | customer_number | int      |
    +-----------------+----------+
    order_number is the primary key for this table.
    This table contains information about the order ID and the customer ID. 

Write an SQL query to find the `customer_number` for the customer who has placed **the largest number of orders**.

The test cases are generated so that **exactly one customer** will have placed more orders than any other customer.

The query result format is in the following example.

**Example 1:**

**Input:**

    Orders table:
    +--------------+-----------------+
    | order_number | customer_number |
    +--------------+-----------------+
    | 1            | 1               |
    | 2            | 2               |
    | 3            | 3               |
    | 4            | 3               |
    +--------------+-----------------+

**Output:**

    +-----------------+
    | customer_number |
    +-----------------+
    | 3               |
    +-----------------+

**Explanation:**

The customer with number 3 has two orders, which is greater than either customer 1 or 2 because each of them only has one order.

So the result is customer\_number 3.

**Follow up:** What if more than one customer has the largest number of orders, can you find all the `customer_number` in this case?

## Solution

```sql
# Write your MySQL query statement below
select customer_number from orders group by customer_number order by count(customer_number) desc limit 1
```