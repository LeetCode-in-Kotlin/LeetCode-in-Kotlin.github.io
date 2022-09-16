[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 45\. Jump Game II

Medium

Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example 1:**

**Input:** nums = [2,3,1,1,4]

**Output:** 2

**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [2,3,0,1,4]

**Output:** 2

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   `0 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun jump(nums: IntArray): Int {
        var length = 0
        var maxLength = 0
        var minJump = 0
        for (i in 0 until nums.size - 1) {
            length--
            maxLength--
            maxLength = Math.max(maxLength, nums[i])
            if (length <= 0) {
                length = maxLength
                minJump++
            }
            if (length >= nums.size - i - 1) {
                return minJump
            }
        }
        return minJump
    }
}
```