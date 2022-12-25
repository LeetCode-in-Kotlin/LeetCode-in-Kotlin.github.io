[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 441\. Arranging Coins

Easy

You have `n` coins and you want to build a staircase with these coins. The staircase consists of `k` rows where the <code>i<sup>th</sup></code> row has exactly `i` coins. The last row of the staircase **may be** incomplete.

Given the integer `n`, return _the number of **complete rows** of the staircase you will build_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins1-grid.jpg)

**Input:** n = 5

**Output:** 2

**Explanation:** Because the 3<sup>rd</sup> row is incomplete, we return 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins2-grid.jpg)

**Input:** n = 8

**Output:** 3

**Explanation:** Because the 4<sup>th</sup> row is incomplete, we return 3.

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun arrangeCoins(n: Int): Int {
        var i = 1
        var x = n
        while (x > 0) {
            x -= ++i
        }
        return i - 1
    }
}
```