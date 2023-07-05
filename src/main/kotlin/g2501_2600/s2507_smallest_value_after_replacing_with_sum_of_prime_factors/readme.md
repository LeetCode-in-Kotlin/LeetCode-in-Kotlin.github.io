[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2507\. Smallest Value After Replacing With Sum of Prime Factors

Medium

You are given a positive integer `n`.

Continuously replace `n` with the sum of its **prime factors**.

*   Note that if a prime factor divides `n` multiple times, it should be included in the sum as many times as it divides `n`.

Return _the smallest value_ `n` _will take on._

**Example 1:**

**Input:** n = 15

**Output:** 5

**Explanation:** Initially, n = 15. 

15 = 3 \* 5, so replace n with 3 + 5 = 8. 

8 = 2 \* 2 \* 2, so replace n with 2 + 2 + 2 = 6.

6 = 2 \* 3, so replace n with 2 + 3 = 5.

5 is the smallest value n will take on.

**Example 2:**

**Input:** n = 3

**Output:** 3

**Explanation:** Initially, n = 3. 3 is the smallest value n will take on.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private val memo: MutableMap<Int, Int> = HashMap()

    fun smallestValue(n: Int): Int {
        var n = n
        while (get(n) != n) {
            n = get(n)
        }
        return n
    }

    private operator fun get(n: Int): Int {
        if (memo.containsKey(n)) {
            return memo[n]!!
        }
        val r = Math.pow(n.toDouble(), 0.5)
        val r1 = r.toInt()
        if (r - r1 == 0.0) {
            return 2 * get(r1)
        }
        var res = 0
        for (i in r1 downTo 2) {
            if (n % i == 0) {
                res = get(i) + get(n / i)
            }
        }
        res = if (res == 0) n else res
        memo[n] = res
        return res
    }
}
```