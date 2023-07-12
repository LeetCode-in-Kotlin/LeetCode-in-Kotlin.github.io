[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2591\. Distribute Money to Maximum Children

Easy

You are given an integer `money` denoting the amount of money (in dollars) that you have and another integer `children` denoting the number of children that you must distribute the money to.

You have to distribute the money according to the following rules:

*   All money must be distributed.
*   Everyone must receive at least `1` dollar.
*   Nobody receives `4` dollars.

Return _the **maximum** number of children who may receive **exactly**_ `8` _dollars if you distribute the money according to the aforementioned rules_. If there is no way to distribute the money, return `-1`.

**Example 1:**

**Input:** money = 20, children = 3

**Output:** 1

**Explanation:** The maximum number of children with 8 dollars will be 1. One of the ways to distribute the money is: 
- 8 dollars to the first child. 
- 9 dollars to the second child. 
- 3 dollars to the third child. 

It can be proven that no distribution exists such that number of children getting 8 dollars is greater than 1.

**Example 2:**

**Input:** money = 16, children = 2

**Output:** 2

**Explanation:** Each child can be given 8 dollars.

**Constraints:**

*   `1 <= money <= 200`
*   `2 <= children <= 30`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun distMoney(money: Int, children: Int): Int {
        var money = money
        if (children > money) return -1
        money -= children
        var ans: Int = money / 7
        if (ans == children && money % 7 == 0) return ans
        if (ans >= children) return children - 1
        if (money % 7 == 3 && ans == children - 1) ans--
        return ans
    }
}
```