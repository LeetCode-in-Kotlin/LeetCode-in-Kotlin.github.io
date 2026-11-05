[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3978\. Unique Middle Element

Easy

You are given an integer array `nums` of odd length `n`.

Return `true` if the middle element of `nums` appears **exactly** once in the array. Otherwise return `false`.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** true

**Explanation:**

The middle element of `nums` is 2, which appears exactly once.

Thus, the answer is `true`.

**Example 2:**

**Input:** nums = [1,2,2]

**Output:** false

**Explanation:**

The middle element of `nums` is 2, which appears twice.

Thus, the answer is `false`.

**Constraints:**

*   `1 <= n == nums.length <= 100`
*   `n` is odd.
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun isMiddleElementUnique(nums: IntArray): Boolean {
        val mid = nums.size / 2
        val key = nums[mid]
        var freq = 0
        for (ele in nums) {
            if (ele == key) {
                freq++
            }
        }
        return (freq == 1)
    }
}
```