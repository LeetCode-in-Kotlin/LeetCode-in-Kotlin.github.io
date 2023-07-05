[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2501\. Longest Square Streak in an Array

Medium

You are given an integer array `nums`. A subsequence of `nums` is called a **square streak** if:

*   The length of the subsequence is at least `2`, and
*   **after** sorting the subsequence, each element (except the first element) is the **square** of the previous number.

Return _the length of the **longest square streak** in_ `nums`_, or return_ `-1` _if there is no **square streak**._

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [4,3,6,16,8,2]

**Output:** 3

**Explanation:** Choose the subsequence [4,16,2]. After sorting it, it becomes [2,4,16].

- 4 = 2 \* 2.

- 16 = 4 \* 4.

Therefore, [4,16,2] is a square streak.

It can be shown that every subsequence of length 4 is not a square streak. 

**Example 2:**

**Input:** nums = [2,3,5,6,7]

**Output:** -1

**Explanation:** There is no square streak in nums so return -1. 

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>2 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun longestSquareStreak(nums: IntArray): Int {
        var result = -1
        val max = 100000
        val isExisted = BooleanArray(max + 1)
        val isVisited = BooleanArray(max + 1)
        for (num in nums) {
            isExisted[num] = true
        }
        var i = 2
        while (i * i <= max) {
            if (!isExisted[i] || isVisited[i]) {
                i++
                continue
            }
            isVisited[i] = true
            var length = 1
            var j = i * i
            while (j >= 0 && j <= max && isExisted[j]) {
                isVisited[j] = true
                length++
                j = j * j
            }
            if (length > 1) {
                result = Math.max(result, length)
            }
            i++
        }
        return result
    }
}
```