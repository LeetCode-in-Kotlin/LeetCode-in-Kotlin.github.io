[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2574\. Left and Right Sum Differences

Easy

Given a **0-indexed** integer array `nums`, find a **0-indexed** integer array `answer` where:

*   `answer.length == nums.length`.
*   `answer[i] = |leftSum[i] - rightSum[i]|`.

Where:

*   `leftSum[i]` is the sum of elements to the left of the index `i` in the array `nums`. If there is no such element, `leftSum[i] = 0`.
*   `rightSum[i]` is the sum of elements to the right of the index `i` in the array `nums`. If there is no such element, `rightSum[i] = 0`.

Return _the array_ `answer`.

**Example 1:**

**Input:** nums = [10,4,8,3]

**Output:** [15,1,11,22]

**Explanation:** The array leftSum is [0,10,14,22] and the array rightSum is [15,11,3,0]. The array answer is [\|0 - 15\|,\|10 - 11\|,\|14 - 3\|,\|22 - 0\|] = [15,1,11,22].

**Example 2:**

**Input:** nums = [1]

**Output:** [0]

**Explanation:** The array leftSum is [0] and the array rightSum is [0]. The array answer is [\|0 - 0\|] = [0].

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun leftRightDifference(nums: IntArray): IntArray {
        val n = nums.size
        val ans = IntArray(n)
        var rightSum = nums.sum()
        var leftSum = 0

        for (i in nums.indices) {
            rightSum = rightSum - nums[i]
            ans[i] = Math.abs(leftSum - rightSum)
            leftSum += nums[i]
        }

        return ans
    }
}
```