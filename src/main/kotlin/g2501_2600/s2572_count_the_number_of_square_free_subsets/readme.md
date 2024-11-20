[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2572\. Count the Number of Square-Free Subsets

Medium

You are given a positive integer **0-indexed** array `nums`.

A subset of the array `nums` is **square-free** if the product of its elements is a **square-free integer**.

A **square-free integer** is an integer that is divisible by no square number other than `1`.

Return _the number of square-free non-empty subsets of the array_ **nums**. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **non-empty** **subset** of `nums` is an array that can be obtained by deleting some (possibly none but not all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [3,4,4,5]

**Output:** 3

**Explanation:** There are 3 square-free subsets in this example: 

- The subset consisting of the 0<sup>th</sup> element [3]. The product of its elements is 3, which is a square-free integer.

- The subset consisting of the 3<sup>rd</sup> element [5]. The product of its elements is 5, which is a square-free integer. 

- The subset consisting of 0<sup>th</sup> and 3<sup>rd</sup> elements [3,5]. The product of its elements is 15, which is a square-free integer. 

It can be proven that there are no more than 3 square-free subsets in the given array.

**Example 2:**

**Input:** nums = [1]

**Output:** 1

**Explanation:** There is 1 square-free subset in this example: 

- The subset consisting of the 0<sup>th</sup> element [1]. The product of its elements is 1, which is a square-free integer. 

It can be proven that there is no more than 1 square-free subset in the given array.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 30`

## Solution

```kotlin
class Solution {
    private val primes = intArrayOf(2, 3, 5, 7, 11, 13, 17, 19, 23, 29)
    private val badNums = (1..30).filter { primes.any { p -> it % (p * p) == 0 } }.toSet()
    private val nonPrimes = (2..30).filter { it !in primes }.filter { it !in badNums }.toList()

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) a else gcd(b, a % b)
    }

    fun squareFreeSubsets(nums: IntArray): Int {
        val mod: Long = 1_000_000_007
        // Get the frequency map
        val freqMap = nums.toTypedArray().groupingBy { it }.eachCount()
        var dp = mutableMapOf<Int, Long>()
        for (v in nonPrimes) {
            if (v !in freqMap) continue
            val howmany = freqMap[v]!!
            val prev = HashMap(dp)
            dp[v] = (dp[v] ?: 0) + howmany
            for ((product, quantity) in prev)
                if (gcd(product, v) == 1) {
                    dp[product * v] = ((dp[product * v] ?: 0L) + quantity * howmany.toLong()) % mod
                }
        }
        for (v in primes) {
            if (v !in freqMap) continue
            val howmany = freqMap[v]!!.toLong()
            val prev = dp
            dp = mutableMapOf<Int, Long>()
            dp[v] = howmany
            for ((product, quantity) in prev)
                dp[product] = if (product % v != 0) quantity * (1 + howmany) else quantity
        }
        // Getting all possible subsets without `1`
        var res = dp.values.sum() % mod
        // Find the permutations of `1`
        val possible = (1..(freqMap[1] ?: 0)).fold(1L) { sum, _ -> (sum shl 1) % mod }
        res = (res * possible + possible + mod - 1) % mod
        return res.toInt()
    }
}
```