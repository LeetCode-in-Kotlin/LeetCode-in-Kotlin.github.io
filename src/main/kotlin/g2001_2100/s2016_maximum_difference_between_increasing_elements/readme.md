[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2016\. Maximum Difference Between Increasing Elements

Easy

Given a **0-indexed** integer array `nums` of size `n`, find the **maximum difference** between `nums[i]` and `nums[j]` (i.e., `nums[j] - nums[i]`), such that `0 <= i < j < n` and `nums[i] < nums[j]`.

Return _the **maximum difference**._ If no such `i` and `j` exists, return `-1`.

**Example 1:**

**Input:** nums = [7,**1**,**5**,4]

**Output:** 4

**Explanation:**

The maximum difference occurs with i = 1 and j = 2, nums[j] - nums[i] = 5 - 1 = 4.

Note that with i = 1 and j = 0, the difference nums[j] - nums[i] = 7 - 1 = 6, but i > j, so it is not valid.

**Example 2:**

**Input:** nums = [9,4,3,2]

**Output:** -1

**Explanation:**

There is no i and j such that i < j and nums[i] < nums[j].

**Example 3:**

**Input:** nums = [**1**,5,2,**10**]

**Output:** 9

**Explanation:**

The maximum difference occurs with i = 0 and j = 3, nums[j] - nums[i] = 10 - 1 = 9.

**Constraints:**

*   `n == nums.length`
*   `2 <= n <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maximumDifference(nums: IntArray): Int {
        var mini = nums[0]
        var ans = -1
        for (i in 0 until nums.size - 1) {
            if (nums[i] < mini) {
                mini = nums[i]
            }
            if (nums[i + 1] - mini > ans) {
                ans = nums[i + 1] - mini
            }
        }
        return if (ans <= 0) -1 else ans
    }
}
```