[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1175\. Prime Arrangements

Easy

Return the number of permutations of 1 to `n` so that prime numbers are at prime indices (1-indexed.)

_(Recall that an integer is prime if and only if it is greater than 1, and cannot be written as a product of two positive integers both smaller than it.)_

Since the answer may be large, return the answer **modulo `10^9 + 7`**.

**Example 1:**

**Input:** n = 5

**Output:** 12

**Explanation:** For example [1,2,5,4,3] is a valid permutation, but [5,2,3,4,1] is not because the prime number 5 is at index 1.

**Example 2:**

**Input:** n = 100

**Output:** 682289015

**Constraints:**

*   `1 <= n <= 100`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun numPrimeArrangements(n: Int): Int {
        var n = n
        val a = intArrayOf(
            2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83,
            89, 97
        )
        var c = 0
        while (c < 25 && n >= a[c]) {
            c++
        }
        val m = 1000000007
        var res = 1L
        while (n - c > 0) {
            res *= (n - c).toLong()
            res %= m.toLong()
            n--
        }
        while (c > 0) {
            res *= c.toLong()
            res %= m.toLong()
            c--
        }
        return res.toInt()
    }
}
```