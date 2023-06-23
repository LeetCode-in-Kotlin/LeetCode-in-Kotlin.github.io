[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1837\. Sum of Digits in Base K

Easy

Given an integer `n` (in base `10`) and a base `k`, return _the **sum** of the digits of_ `n` _**after** converting_ `n` _from base_ `10` _to base_ `k`.

After converting, each digit should be interpreted as a base `10` number, and the sum should be returned in base `10`.

**Example 1:**

**Input:** n = 34, k = 6

**Output:** 9

**Explanation:** 34 (base 10) expressed in base 6 is 54. 5 + 4 = 9.

**Example 2:**

**Input:** n = 10, k = 10

**Output:** 1

**Explanation:** n is already in base 10. 1 + 0 = 1.

**Constraints:**

*   `1 <= n <= 100`
*   `2 <= k <= 10`

## Solution

```kotlin
class Solution {
    fun sumBase(n: Int, k: Int): Int {
        val str = (n.toString() + "").toInt(10).toString(k)
        var sum = 0
        for (c in str.toCharArray()) {
            sum += Character.getNumericValue(c)
        }
        return sum
    }
}
```