[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1685\. Sum of Absolute Differences in a Sorted Array

Medium

You are given an integer array `nums` sorted in **non-decreasing** order.

Build and return _an integer array_ `result` _with the same length as_ `nums` _such that_ `result[i]` _is equal to the **summation of absolute differences** between_ `nums[i]` _and all the other elements in the array._

In other words, `result[i]` is equal to `sum(|nums[i]-nums[j]|)` where `0 <= j < nums.length` and `j != i` (**0-indexed**).

**Example 1:**

**Input:** nums = [2,3,5]

**Output:** [4,3,5]

**Explanation:** Assuming the arrays are 0-indexed, then

result[0] = \|2-2\| + \|2-3\| + \|2-5\| = 0 + 1 + 3 = 4,

result[1] = \|3-2\| + \|3-3\| + \|3-5\| = 1 + 0 + 2 = 3,

result[2] = \|5-2\| + \|5-3\| + \|5-5\| = 3 + 2 + 0 = 5.

**Example 2:**

**Input:** nums = [1,4,6,8,10]

**Output:** [24,15,13,15,21]

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= nums[i + 1] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun getSumAbsoluteDifferences(nums: IntArray): IntArray {
        val len = nums.size
        val preSums = IntArray(len)
        for (i in 1 until len) {
            preSums[i] = preSums[i - 1] + nums[i - 1]
        }
        val postSums = IntArray(len)
        for (i in len - 2 downTo 0) {
            postSums[i] = postSums[i + 1] + nums[i + 1]
        }
        val result = IntArray(len)
        for (i in 0 until len) {
            result[i] = nums[i] * i - preSums[i] + postSums[i] - nums[i] * (len - i - 1)
        }
        return result
    }
}
```