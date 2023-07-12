[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2563\. Count the Number of Fair Pairs

Medium

Given a **0-indexed** integer array `nums` of size `n` and two integers `lower` and `upper`, return _the number of fair pairs_.

A pair `(i, j)` is **fair** if:

*   `0 <= i < j < n`, and
*   `lower <= nums[i] + nums[j] <= upper`

**Example 1:**

**Input:** nums = [0,1,7,4,4,5], lower = 3, upper = 6

**Output:** 6

**Explanation:** There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).

**Example 2:**

**Input:** nums = [1,7,9,2,5], lower = 11, upper = 11

**Output:** 1

**Explanation:** There is a single fair pair: (2,3).

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums.length == n`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>-10<sup>9</sup> <= lower <= upper <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countFairPairs(nums: IntArray, lower: Int, upper: Int): Long {
        nums.sort()
        return smaller(nums, upper) - smaller(nums, lower - 1)
    }

    private fun smaller(nums: IntArray, value: Int): Long {
        var l = 0
        var r = nums.size - 1
        var result: Long = 0
        while (l < r) {
            val sum = nums[l] + nums[r]
            if (sum <= value) {
                result += (r - l).toLong()
                l++
            } else {
                r--
            }
        }
        return result
    }
}
```