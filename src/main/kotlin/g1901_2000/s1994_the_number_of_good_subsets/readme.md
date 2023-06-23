[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1994\. The Number of Good Subsets

Hard

You are given an integer array `nums`. We call a subset of `nums` **good** if its product can be represented as a product of one or more **distinct prime** numbers.

*   For example, if `nums = [1, 2, 3, 4]`:
    *   `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are **good** subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
    *   `[1, 4]` and `[4]` are not **good** subsets with products `4 = 2*2` and `4 = 2*2` respectively.

Return _the number of different **good** subsets in_ `nums` _**modulo**_ <code>10<sup>9</sup> + 7</code>.

A **subset** of `nums` is any array that can be obtained by deleting some (possibly none or all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** 6

**Explanation:** The good subsets are:

- \[1,2]: product is 2, which is the product of distinct prime 2.

- \[1,2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[1,3]: product is 3, which is the product of distinct prime 3.

- \[2]: product is 2, which is the product of distinct prime 2.

- \[2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[3]: product is 3, which is the product of distinct prime 3. 

**Example 2:**

**Input:** nums = [4,2,3,15]

**Output:** 5

**Explanation:** The good subsets are:

- \[2]: product is 2, which is the product of distinct prime 2.

- \[2,3]: product is 6, which is the product of distinct primes 2 and 3.

- \[2,15]: product is 30, which is the product of distinct primes 2, 3, and 5.

- \[3]: product is 3, which is the product of distinct prime 3.

- \[15]: product is 15, which is the product of distinct primes 3 and 5. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= 30`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun add(a: Long, b: Long): Long {
        var a = a
        a += b
        return if (a < MOD) a else a - MOD
    }

    private fun mul(a: Long, b: Long): Long {
        var a = a
        a *= b
        return if (a < MOD) a else a % MOD
    }

    private fun pow(a: Long, b: Long): Long {
        // a %= MOD;
        // b%=(MOD-1);//if MOD is prime
        var a = a
        var b = b
        var res: Long = 1
        while (b > 0) {
            if (b and 1L == 1L) {
                res = mul(res, a)
            }
            a = mul(a, a)
            b = b shr 1
        }
        return add(res, 0)
    }

    fun numberOfGoodSubsets(nums: IntArray): Int {
        val primes = intArrayOf(2, 3, 5, 7, 11, 13, 17, 19, 23, 29)
        val mask = IntArray(31)
        val freq = IntArray(31)
        for (x in nums) {
            freq[x]++
        }
        for (i in 1..30) {
            for (j in primes.indices) {
                if (i % primes[j] == 0) {
                    if (i / primes[j] % primes[j] == 0) {
                        mask[i] = 0
                        break
                    }
                    mask[i] = mask[i] or pow(2, j.toLong()).toInt()
                }
            }
        }
        val dp = LongArray(1024)
        dp[0] = 1
        for (i in 1..30) {
            if (mask[i] != 0) {
                for (j in 0..1023) {
                    if (mask[i] and j == 0 && dp[j] > 0) {
                        dp[mask[i] or j] = add(dp[mask[i] or j], mul(dp[j], freq[i].toLong()))
                    }
                }
            }
        }
        var ans: Long = 0
        for (i in 1..1023) {
            ans = add(ans, dp[i])
        }
        ans = mul(ans, pow(2, freq[1].toLong()))
        ans = add(ans, 0)
        return ans.toInt()
    }

    companion object {
        private const val MOD = (1e9 + 7).toLong()
    }
}
```