[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1716\. Calculate Money in Leetcode Bank

Easy

Hercy wants to save money for his first car. He puts money in the Leetcode bank **every day**.

He starts by putting in `$1` on Monday, the first day. Every day from Tuesday to Sunday, he will put in `$1` more than the day before. On every subsequent Monday, he will put in `$1` more than the **previous Monday**.

Given `n`, return _the total amount of money he will have in the Leetcode bank at the end of the_ <code>n<sup>th</sup></code> _day._

**Example 1:**

**Input:** n = 4

**Output:** 10

**Explanation:** After the 4<sup>th</sup> day, the total is 1 + 2 + 3 + 4 = 10.

**Example 2:**

**Input:** n = 10

**Output:** 37

**Explanation:** After the 10<sup>th</sup> day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37. Notice that on the 2<sup>nd</sup> Monday, Hercy only puts in $2.

**Example 3:**

**Input:** n = 20

**Output:** 96

**Explanation:** After the 20<sup>th</sup> day, the total is (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun totalMoney(n: Int): Int {
        var n = n
        var mondayMoney = 1
        var total = 0
        while (n > 0) {
            var weekDays = 0
            var base = mondayMoney
            while (weekDays < 7 && n > 0) {
                total += base
                base++
                weekDays++
                n--
            }
            mondayMoney++
        }
        return total
    }
}
```