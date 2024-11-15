[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3351\. Sum of Good Subsequences

Hard

You are given an integer array `nums`. A **good** subsequence is defined as a subsequence of `nums` where the absolute difference between any **two** consecutive elements in the subsequence is **exactly** 1.

Return the **sum** of all _possible_ **good subsequences** of `nums`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note** that a subsequence of size 1 is considered good by definition.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** 14

**Explanation:**

*   Good subsequences are: `[1]`, `[2]`, `[1]`, `[1,2]`, `[2,1]`, `[1,2,1]`.
*   The sum of elements in these subsequences is 14.

**Example 2:**

**Input:** nums = [3,4,5]

**Output:** 40

**Explanation:**

*   Good subsequences are: `[3]`, `[4]`, `[5]`, `[3,4]`, `[4,5]`, `[3,4,5]`.
*   The sum of elements in these subsequences is 40.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun sumOfGoodSubsequences(nums: IntArray): Int {
        var max = 0
        for (x in nums) {
            max = max(x, max)
        }
        val count = LongArray(max + 3)
        val total = LongArray(max + 3)
        val mod = (1e9 + 7).toInt().toLong()
        var res: Long = 0
        for (a in nums) {
            count[a + 1] = (count[a] + count[a + 1] + count[a + 2] + 1) % mod
            val cur = total[a] + total[a + 2] + a * (count[a] + count[a + 2] + 1)
            total[a + 1] = (total[a + 1] + cur) % mod
            res = (res + cur) % mod
        }
        return res.toInt()
    }
}
```