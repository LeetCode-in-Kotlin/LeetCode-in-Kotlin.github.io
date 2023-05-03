[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 932\. Beautiful Array

Medium

An array `nums` of length `n` is **beautiful** if:

*   `nums` is a permutation of the integers in the range `[1, n]`.
*   For every `0 <= i < j < n`, there is no index `k` with `i < k < j` where `2 * nums[k] == nums[i] + nums[j]`.

Given the integer `n`, return _any **beautiful** array_ `nums` _of length_ `n`. There will be at least one valid answer for the given `n`.

**Example 1:**

**Input:** n = 4

**Output:** [2,1,4,3]

**Example 2:**

**Input:** n = 5

**Output:** [3,1,2,5,4]

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
class Solution {
    private var memo: MutableMap<Int, IntArray>? = null
    fun beautifulArray(n: Int): IntArray? {
        memo = HashMap()
        return helper(n)
    }

    private fun helper(n: Int): IntArray? {
        if (n == 1) {
            memo!![1] = intArrayOf(1)
            return intArrayOf(1)
        }
        if (memo!!.containsKey(n)) {
            return memo!![n]
        }
        val mid = (n + 1) / 2
        val left = helper(mid)
        val right = helper(n - mid)
        val rst = IntArray(n)
        for (i in 0 until mid) {
            rst[i] = left!![i] * 2 - 1
        }
        for (i in mid until n) {
            rst[i] = right!![i - mid] * 2
        }
        memo!![n] = rst
        return rst
    }
}
```