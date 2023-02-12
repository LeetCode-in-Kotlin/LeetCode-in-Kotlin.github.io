[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 629\. K Inverse Pairs Array

Hard

For an integer array `nums`, an **inverse pair** is a pair of integers `[i, j]` where `0 <= i < j < nums.length` and `nums[i] > nums[j]`.

Given two integers n and k, return the number of different arrays consist of numbers from `1` to `n` such that there are exactly `k` **inverse pairs**. Since the answer can be huge, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, k = 0

**Output:** 1

**Explanation:** Only the array [1,2,3] which consists of numbers from 1 to 3 has exactly 0 inverse pairs.

**Example 2:**

**Input:** n = 3, k = 1

**Output:** 2

**Explanation:** The array [1,3,2] and [2,1,3] have exactly 1 inverse pair.

**Constraints:**

*   `1 <= n <= 1000`
*   `0 <= k <= 1000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun kInversePairs(n: Int, k: Int): Int {
        var k = k
        k = Math.min(k, n * (n - 1) / 2 - k)
        if (k < 0) {
            return 0
        }
        var dp = IntArray(k + 1)
        var dp1 = IntArray(k + 1)
        dp[0] = 1
        dp1[0] = 1
        val mod = 1000000007
        for (i in 1..n) {
            val temp = dp
            dp = dp1
            dp1 = temp
            var j = 1
            val m = Math.min(k, i * (i - 1) / 2)
            while (j <= m) {
                dp[j] = (dp1[j] + dp[j - 1] - if (j >= i) dp1[j - i] else 0) % mod
                if (dp[j] < 0) {
                    dp[j] += mod
                }
                j++
            }
        }
        return dp[k]
    }
}
```