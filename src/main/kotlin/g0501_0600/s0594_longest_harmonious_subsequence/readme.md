[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 594\. Longest Harmonious Subsequence

Easy

We define a harmonious array as an array where the difference between its maximum value and its minimum value is **exactly** `1`.

Given an integer array `nums`, return _the length of its longest harmonious subsequence among all its possible subsequences_.

A **subsequence** of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [1,3,2,2,5,2,3,7]

**Output:** 5

**Explanation:** The longest harmonious subsequence is [3,2,2,2,3].

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 2

**Example 3:**

**Input:** nums = [1,1,1,1]

**Output:** 0

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun findLHS(nums: IntArray): Int {
        Arrays.sort(nums)
        var max = 0
        var lastN = 0
        var curN = 1
        var cur = nums[0]
        for (i in 1 until nums.size) {
            if (nums[i] > cur) {
                if (lastN > 0 && curN > 0 && lastN + curN > max) {
                    max = lastN + curN
                }
                // if diff more than 1, don't count
                lastN = if (nums[i] - cur == 1) curN else 0
                curN = 1
                cur = nums[i]
            } else {
                curN++
            }
        }
        if (lastN > 0 && curN > 0 && lastN + curN > max) {
            max = lastN + curN
        }
        return max
    }
}
```