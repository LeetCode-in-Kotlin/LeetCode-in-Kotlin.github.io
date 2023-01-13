[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 493\. Reverse Pairs

Hard

Given an integer array `nums`, return _the number of **reverse pairs** in the array_.

A **reverse pair** is a pair `(i, j)` where:

*   `0 <= i < j < nums.length` and
*   `nums[i] > 2 * nums[j]`.

**Example 1:**

**Input:** nums = [1,3,2,3,1]

**Output:** 2

**Explanation:** The reverse pairs are: 

(1, 4) --> nums[1] = 3, nums[4] = 1, 3 > 2 \* 1 

(3, 4) --> nums[3] = 3, nums[4] = 1, 3 > 2 \* 1

**Example 2:**

**Input:** nums = [2,4,3,5,1]

**Output:** 3

**Explanation:** The reverse pairs are: 

(1, 4) --> nums[1] = 4, nums[4] = 1, 4 > 2 \* 1 

(2, 4) --> nums[2] = 3, nums[4] = 1, 3 > 2 \* 1 

(3, 4) --> nums[3] = 5, nums[4] = 1, 5 > 2 \* 1

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun reversePairs(nums: IntArray): Int {
        return mergeSort(nums, 0, nums.size - 1)
    }

    private fun mergeSort(nums: IntArray, start: Int, end: Int): Int {
        if (start >= end) {
            return 0
        }
        val mid = start + (end - start) / 2
        var cnt = mergeSort(nums, start, mid) + mergeSort(nums, mid + 1, end)
        var j = mid + 1
        for (i in start..mid) {
            // it has to be 2.0 instead of 2, otherwise it's going to stack overflow, i.e. test3 is
            // going to fail
            while (j <= end && nums[i] > nums[j] * 2.0) {
                j++
            }
            cnt += j - (mid + 1)
        }
        Arrays.sort(nums, start, end + 1)
        return cnt
    }
}
```