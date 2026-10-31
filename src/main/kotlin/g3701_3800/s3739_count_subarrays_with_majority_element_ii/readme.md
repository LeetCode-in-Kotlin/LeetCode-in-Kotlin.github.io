[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3739\. Count Subarrays With Majority Element II

Hard

You are given an integer array `nums` and an integer `target`.

Return the number of **non-empty subarrays** of `nums` in which `target` is the **majority element**.

The **majority element** of a subarray is the element that appears **strictly more than half** of the times in that subarray.

**Example 1:**

**Input:** nums = [1,2,2,3], target = 2

**Output:** 5

**Explanation:**

Valid subarrays with `target = 2` as the majority element:

*   `nums[1..1] = [2]`
*   `nums[2..2] = [2]`
*   `nums[1..2] = [2,2]`
*   `nums[0..2] = [1,2,2]`
*   `nums[1..3] = [2,2,3]`

So there are 5 such subarrays.

**Example 2:**

**Input:** nums = [1,1,1,1], target = 1

**Output:** 10

**Explanation:**

All 10 subarrays have 1 as the majority element.

**Example 3:**

**Input:** nums = [1,2,3], target = 4

**Output:** 0

**Explanation:**

`target = 4` does not appear in `nums` at all. Therefore, there cannot be any subarray where 4 is the majority element. Hence the answer is 0.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= target <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countMajoritySubarrays(nums: IntArray, target: Int): Long {
        val n = nums.size
        var pre = n + 1
        val count = LongArray(2 * n + 2)
        val acc = LongArray(2 * n + 2)
        var res: Long = 0
        acc[pre] = 1
        count[pre] = acc[pre]
        for (a in nums) {
            pre += (if (a == target) 1 else -1)
            acc[pre] = ++count[pre] + acc[pre - 1]
            res += acc[pre - 1]
        }
        return res
    }
}
```