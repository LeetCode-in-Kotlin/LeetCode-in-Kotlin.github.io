[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3918\. Sum of Primes Between Number and Its Reverse

Medium

You are given an integer `n`.

Let `r` be the integer formed by reversing the digits of `n`.

Return the **sum** of all prime numbers between `min(n, r)` and `max(n, r)`, inclusive.

**Example 1:**

**Input:** n = 13

**Output:** 132

**Explanation:**

*   The reverse of 13 is 31. Thus, the range is `[13, 31]`.
*   The prime numbers in this range are 13, 17, 19, 23, 29, and 31.
*   The sum of these prime numbers is `13 + 17 + 19 + 23 + 29 + 31 = 132`.

**Example 2:**

**Input:** n = 10

**Output:** 17

**Explanation:**

*   The reverse of 10 is 1. Thus, the range is `[1, 10]`.
*   The prime numbers in this range are 2, 3, 5, and 7.
*   The sum of these prime numbers is `2 + 3 + 5 + 7 = 17`.

**Example 3:**

**Input:** n = 8

**Output:** 0

**Explanation:**

*   The reverse of 8 is 8. Thus, the range is `[8, 8]`.
*   There are no prime numbers in this range, so the sum is 0.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    private fun isPrime(x: Int): Boolean {
        if (x <= 1) {
            return false
        }
        if (x == 2) {
            return true
        }
        if (x % 2 == 0) {
            return false
        }
        var i = 3
        while (i * i <= x) {
            if (x % i == 0) {
                return false
            }
            i += 2
        }
        return true
    }

    private fun reverseNum(n: Int): Int {
        var n = n
        var r = 0
        while (n > 0) {
            r = r * 10 + (n % 10)
            n /= 10
        }
        return r
    }

    fun sumOfPrimesInRange(n: Int): Int {
        val r = reverseNum(n)
        val low = min(n, r)
        val high = max(n, r)
        var sum = 0
        for (i in low..high) {
            if (isPrime(i)) {
                sum += i
            }
        }
        return sum
    }
}
```