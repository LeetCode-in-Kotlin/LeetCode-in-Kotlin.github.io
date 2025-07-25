[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3584\. Maximum Product of First and Last Elements of a Subsequence

Medium

You are given an integer array `nums` and an integer `m`.

Return the **maximum** product of the first and last elements of any ****subsequences**** of `nums` of size `m`.

**Example 1:**

**Input:** nums = [-1,-9,2,3,-2,-3,1], m = 1

**Output:** 81

**Explanation:**

The subsequence `[-9]` has the largest product of the first and last elements: `-9 * -9 = 81`. Therefore, the answer is 81.

**Example 2:**

**Input:** nums = [1,3,-5,5,6,-4], m = 3

**Output:** 20

**Explanation:**

The subsequence `[-5, 6, -4]` has the largest product of the first and last elements.

**Example 3:**

**Input:** nums = [2,-1,2,-6,5,2,-5,7], m = 2

**Output:** 35

**Explanation:**

The subsequence `[5, 7]` has the largest product of the first and last elements.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= m <= nums.length`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maximumProduct(nums: IntArray, m: Int): Long {
        var ma = nums[0].toLong()
        var mi = nums[0].toLong()
        var res = nums[0].toLong() * nums[m - 1]
        for (i in m - 1..<nums.size) {
            ma = max(ma, nums[i - m + 1].toLong())
            mi = min(mi, nums[i - m + 1].toLong())
            res = max(res, max(mi * nums[i], ma * nums[i]))
        }
        return res
    }
}
```