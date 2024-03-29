[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 509\. Fibonacci Number

Easy

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

F(0) = 0, F(1) = 1 F(n) = F(n - 1) + F(n - 2), for n > 1.

Given `n`, calculate `F(n)`.

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:** F(2) = F(1) + F(0) = 1 + 0 = 1.

**Example 2:**

**Input:** n = 3

**Output:** 2

**Explanation:** F(3) = F(2) + F(1) = 1 + 1 = 2.

**Example 3:**

**Input:** n = 4

**Output:** 3

**Explanation:** F(4) = F(3) + F(2) = 2 + 1 = 3.

**Constraints:**

*   `0 <= n <= 30`

## Solution

```kotlin
class Solution {
    private val memo = IntArray(31)

    fun fib(n: Int): Int {
        if (n == 0) {
            return 0
        }
        if (n == 1) {
            return 1
        }
        if (memo[n] != 0) {
            return memo[n]
        }
        memo[n] = fib(n - 1) + fib(n - 2)
        return memo[n]
    }
}
```