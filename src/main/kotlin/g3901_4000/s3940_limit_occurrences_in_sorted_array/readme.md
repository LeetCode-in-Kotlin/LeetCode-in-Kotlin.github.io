[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3940\. Limit Occurrences in Sorted Array

Easy

You are given a **sorted** integer array `nums` and an integer `k`.

Return an array such that each **distinct** element appears **at most** `k` times, while preserving the relative order of the elements in `nums`.

Note: If a distinct element appears **at least** `k` times, then it must appear **exactly** `k` times in the resulting array.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2

**Output:** [1,1,2,2,3]

**Explanation:**

Each element can appear at most 2 times.

*   The element 1 appears 3 times, so only 2 occurrences are kept.
*   The element 2 appears 2 times, so both occurrences are kept.
*   The element 3 appears 1 time, so it is kept.

Thus, the resulting array is `[1, 1, 2, 2, 3]`.

**Example 2:**

**Input:** nums = [1,2,3], k = 1

**Output:** [1,2,3]

**Explanation:**

All elements are distinct and already appear at most once, so the array remains unchanged.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`
*   `nums` is sorted in non-decreasing order.
*   `1 <= k <= nums.length`

**Follow-up:**

*   Can you solve this in-place using O(1) extra space?
*   Note that the space used for returning or resizing the result does not count toward the space complexity mentioned above, as some languages do not support in-place resizing.

## Solution

```kotlin
class Solution {
    fun limitOccurrences(nums: IntArray, k: Int): IntArray {
        val ls: MutableList<Int?> = ArrayList<Int?>()
        ls.add(nums[0])
        var freq = 1
        for (i in 1..<nums.size) {
            if (nums[i] == nums[i - 1]) {
                freq++
            } else {
                freq = 1
            }
            if (freq <= k) {
                ls.add(nums[i])
            }
        }
        val ans = IntArray(ls.size)
        for (i in ls.indices) {
            ans[i] = ls[i]!!
        }
        return ans
    }
}
```