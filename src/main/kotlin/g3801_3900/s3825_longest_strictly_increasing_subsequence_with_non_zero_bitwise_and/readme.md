[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3825\. Longest Strictly Increasing Subsequence With Non-Zero Bitwise AND

Medium

You are given an integer array `nums`.

Return the length of the **longest strictly increasing subsequence** in `nums` whose bitwise **AND** is **non-zero**. If no such **subsequence** exists, return 0.

**Example 1:**

**Input:** nums = [5,4,7]

**Output:** 2

**Explanation:**

One longest strictly increasing subsequence is `[5, 7]`. The bitwise AND is `5 AND 7 = 5`, which is non-zero.

**Example 2:**

**Input:** nums = [2,3,6]

**Output:** 3

**Explanation:**

The longest strictly increasing subsequence is `[2, 3, 6]`. The bitwise AND is `2 AND 3 AND 6 = 2`, which is non-zero.

**Example 3:**

**Input:** nums = [0,1]

**Output:** 1

**Explanation:**

One longest strictly increasing subsequence is `[1]`. The bitwise AND is 1, which is non-zero.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private fun lis(nums: IntArray, n: Int): Int {
        val tails = IntArray(n)
        var sz = 0
        for (i in 0..<n) {
            val x = nums[i]
            var l = 0
            var r = sz
            while (l < r) {
                val m = (l + r) ushr 1
                if (tails[m] >= x) {
                    r = m
                } else {
                    l = m + 1
                }
            }
            tails[l] = x
            if (l == sz) {
                sz++
            }
        }
        return sz
    }

    fun longestSubsequence(nums: IntArray): Int {
        val maxbits = 32
        var result = 0
        val n = nums.size
        val buf = IntArray(n)
        for (bit in 0..<maxbits) {
            var m = 0
            for (x in nums) {
                if (((x shr bit) and 1) != 0) {
                    buf[m++] = x
                }
            }
            result = max(result, lis(buf, m))
        }
        return result
    }
}
```