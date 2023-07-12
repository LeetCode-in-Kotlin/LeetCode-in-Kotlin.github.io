[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2597\. The Number of Beautiful Subsets

Medium

You are given an array `nums` of positive integers and a **positive** integer `k`.

A subset of `nums` is **beautiful** if it does not contain two integers with an absolute difference equal to `k`.

Return _the number of **non-empty beautiful** subsets of the array_ `nums`.

A **subset** of `nums` is an array that can be obtained by deleting some (possibly none) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [2,4,6], k = 2

**Output:** 4

**Explanation:**

The beautiful subsets of the array nums are: [2], [4], [6], [2, 6].

It can be proved that there are only 4 beautiful subsets in the array [2,4,6].

**Example 2:**

**Input:** nums = [1], k = 1

**Output:** 1

**Explanation:**

The beautiful subset of the array nums is [1].

It can be proved that there is only 1 beautiful subset in the array [1].

**Constraints:**

*   `1 <= nums.length <= 20`
*   `1 <= nums[i], k <= 1000`

## Solution

```kotlin
class Solution {
    fun beautifulSubsets(nums: IntArray, k: Int): Int {
        val map: MutableMap<Int, Int> = HashMap()
        for (n in nums) {
            map[n] = map.getOrDefault(n, 0) + 1
        }
        var res = 1
        for (key in map.keys) {
            if (!map.containsKey(key - k)) {
                if (!map.containsKey(key + k)) {
                    res *= 1 shl map[key]!!
                } else {
                    val freq: MutableList<Int?> = ArrayList()
                    var localKey = key
                    while (map.containsKey(localKey)) {
                        freq.add(map[localKey])
                        localKey += k
                    }
                    res *= helper(freq)
                }
            }
        }
        return res - 1
    }

    private fun helper(freq: List<Int?>): Int {
        val n = freq.size
        if (n == 1) {
            return 1 shl freq[0]!!
        }
        val dp = IntArray(n)
        dp[0] = (1 shl freq[0]!!) - 1
        dp[1] = (1 shl freq[1]!!) - 1
        if (n == 2) {
            return dp[0] + dp[1] + 1
        }
        for (i in 2 until n) {
            if (i > 2) {
                dp[i - 2] += dp[i - 3]
            }
            dp[i] = (dp[i - 2] + 1) * ((1 shl freq[i]!!) - 1)
        }
        return dp[n - 1] + dp[n - 2] + dp[n - 3] + 1
    }
}
```