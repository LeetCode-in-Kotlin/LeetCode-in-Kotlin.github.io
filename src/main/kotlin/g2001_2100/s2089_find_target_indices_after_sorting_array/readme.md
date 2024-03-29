[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2089\. Find Target Indices After Sorting Array

Easy

You are given a **0-indexed** integer array `nums` and a target element `target`.

A **target index** is an index `i` such that `nums[i] == target`.

Return _a list of the target indices of_ `nums` after _sorting_ `nums` _in **non-decreasing** order_. If there are no target indices, return _an **empty** list_. The returned list must be sorted in **increasing** order.

**Example 1:**

**Input:** nums = [1,2,5,2,3], target = 2

**Output:** [1,2]

**Explanation:** After sorting, nums is [1,**2**,**2**,3,5].

The indices where nums[i] == 2 are 1 and 2. 

**Example 2:**

**Input:** nums = [1,2,5,2,3], target = 3

**Output:** [3]

**Explanation:** After sorting, nums is [1,2,2,**3**,5].

The index where nums[i] == 3 is 3. 

**Example 3:**

**Input:** nums = [1,2,5,2,3], target = 5

**Output:** [4]

**Explanation:** After sorting, nums is [1,2,2,3,**5**].

The index where nums[i] == 5 is 4. 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i], target <= 100`

## Solution

```kotlin
class Solution {
    fun targetIndices(nums: IntArray, target: Int): List<Int> {
        var count = 0
        var lessthan = 0
        for (n in nums) {
            if (n == target) {
                count++
            }
            if (n < target) {
                lessthan++
            }
        }
        val result: MutableList<Int> = ArrayList()
        for (i in 0 until count) {
            result.add(lessthan++)
        }
        return result
    }
}
```