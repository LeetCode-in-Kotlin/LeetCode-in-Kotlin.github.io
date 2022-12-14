[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 324\. Wiggle Sort II

Medium

Given an integer array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`.

You may assume the input array always has a valid answer.

**Example 1:**

**Input:** nums = [1,5,1,1,6,4]

**Output:** [1,6,1,5,1,4]

**Explanation:** [1,4,1,5,1,6] is also accepted.

**Example 2:**

**Input:** nums = [1,3,2,2,3,1]

**Output:** [2,3,1,3,1,2]

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   `0 <= nums[i] <= 5000`
*   It is guaranteed that there will be an answer for the given input `nums`.

**Follow Up:** Can you do it in `O(n)` time and/or **in-place** with `O(1)` extra space?

## Solution

```kotlin
class Solution {
    fun wiggleSort(nums: IntArray) {
        nums.sort()
        val result = IntArray(nums.size)
        var index = nums.size - 1
        var i = 1
        // Start filling all peaks (which is all at odd indexes) from start
        while (i < nums.size) {
            result[i] = nums[index]
            --index
            i += 2
        }
        // Start filling all valleys (which is all at even indexes) from end
        // why from end, as the last peak index may have smallest largest value, so to
        // make sure, that is also '>', fill in the smallest element near it.
        i = if ((nums.size - 1) % 2 == 0) nums.size - 1 else nums.size - 2
        index = 0
        while (i >= 0) {
            result[i] = nums[index]
            ++index
            i -= 2
        }
        System.arraycopy(result, 0, nums, 0, nums.size)
    }
}
```