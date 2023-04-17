[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 905\. Sort Array By Parity

Easy

Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.

Return _**any array** that satisfies this condition_.

**Example 1:**

**Input:** nums = [3,1,2,4]

**Output:** [2,4,3,1]

**Explanation:** The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

**Example 2:**

**Input:** nums = [0]

**Output:** [0]

**Constraints:**

*   `1 <= nums.length <= 5000`
*   `0 <= nums[i] <= 5000`

## Solution

```kotlin
class Solution {
    fun sortArrayByParity(nums: IntArray): IntArray {
        var temp: Int
        var i = 0
        for (k in nums.indices) {
            if (nums[k] % 2 == 0) {
                temp = nums[k]
                nums[k] = nums[i]
                nums[i] = temp
                i++
            }
        }
        return nums
    }
}
```