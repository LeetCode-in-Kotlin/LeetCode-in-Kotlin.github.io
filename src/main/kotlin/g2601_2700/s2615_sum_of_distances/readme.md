[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2615\. Sum of Distances

Medium

You are given a **0-indexed** integer array `nums`. There exists an array `arr` of length `nums.length`, where `arr[i]` is the sum of `|i - j|` over all `j` such that `nums[j] == nums[i]` and `j != i`. If there is no such `j`, set `arr[i]` to be `0`.

Return _the array_ `arr`_._

**Example 1:**

**Input:** nums = [1,3,1,1,2]

**Output:** [5,0,3,4,0]

**Explanation:**

When i = 0, nums[0] == nums[2] and nums[0] == nums[3]. Therefore, arr[0] = \|0 - 2\| + \|0 - 3\| = 5.

When i = 1, arr[1] = 0 because there is no other index with value 3.

When i = 2, nums[2] == nums[0] and nums[2] == nums[3]. Therefore, arr[2] = \|2 - 0\| + \|2 - 3\| = 3.

When i = 3, nums[3] == nums[0] and nums[3] == nums[2]. Therefore, arr[3] = \|3 - 0\| + \|3 - 2\| = 4.

When i = 4, arr[4] = 0 because there is no other index with value 2.

**Example 2:**

**Input:** nums = [0,5,3]

**Output:** [0,0,0]

**Explanation:** Since each element in nums is distinct, arr[i] = 0 for all i.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun distance(nums: IntArray): LongArray {
        val map = HashMap<Int, LongArray>()
        for (i in nums.indices) {
            var temp = map[nums[i]]
            if (temp == null) {
                temp = LongArray(4)
                map[nums[i]] = temp
            }
            temp[0] += i.toLong()
            temp[2] += 1L
        }
        val ans = LongArray(nums.size)
        for (i in nums.indices) {
            val temp = map[nums[i]]
            ans[i] += i * temp!![3] - temp[1]
            temp[1] += i.toLong()
            temp[3] += 1L
            ans[i] += temp[0] - temp[1] - i * (temp[2] - temp[3])
        }
        return ans
    }
}
```