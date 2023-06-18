[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1793\. Maximum Score of a Good Subarray

Hard

You are given an array of integers `nums` **(0-indexed)** and an integer `k`.

The **score** of a subarray `(i, j)` is defined as `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A **good** subarray is a subarray where `i <= k <= j`.

Return _the maximum possible **score** of a **good** subarray._

**Example 1:**

**Input:** nums = [1,4,3,7,4,5], k = 3

**Output:** 15

**Explanation:** The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) \* (5-1+1) = 3 \* 5 = 15. 

**Example 2:**

**Input:** nums = [5,5,4,5,4,1,1,1], k = 0

**Output:** 20

**Explanation:** The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) \* (4-0+1) = 4 \* 5 = 20. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   `0 <= k < nums.length`

## Solution

```kotlin
class Solution {
    fun maximumScore(nums: IntArray, k: Int): Int {
        var i = k
        var j = k
        var res = nums[k]
        var min = nums[k]
        var goLeft: Boolean
        while (i >= 1 || j < nums.size - 1) {
            // sub array [i...j] is already traversed. Either goLeft or goRight to increase the
            // sequence
            goLeft = if (i == 0) {
                false
            } else if (j == nums.size - 1) {
                true
            } else {
                nums[j + 1] <= nums[i - 1]
            }
            min = if (goLeft) Math.min(min, nums[i - 1]) else Math.min(min, nums[j + 1])
            if (goLeft) {
                while (i >= 1 && min <= nums[i - 1]) {
                    i--
                }
            } else {
                while (j < nums.size - 1 && min <= nums[j + 1]) {
                    j++
                }
            }
            res = Math.max(res, min * (j - i + 1))
        }
        return res
    }
}
```