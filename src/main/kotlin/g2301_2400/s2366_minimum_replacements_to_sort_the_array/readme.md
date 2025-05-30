[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2366\. Minimum Replacements to Sort the Array

Hard

You are given a **0-indexed** integer array `nums`. In one operation you can replace any element of the array with **any two** elements that **sum** to it.

*   For example, consider `nums = [5,6,7]`. In one operation, we can replace `nums[1]` with `2` and `4` and convert `nums` to `[5,2,4,7]`.

Return _the minimum number of operations to make an array that is sorted in **non-decreasing** order_.

**Example 1:**

**Input:** nums = [3,9,3]

**Output:** 2

**Explanation:** Here are the steps to sort the array in non-decreasing order: 

- From [3,9,3], replace the 9 with 3 and 6 so the array becomes [3,3,6,3] 

- From [3,3,6,3], replace the 6 with 3 and 3 so the array becomes [3,3,3,3,3] 
  
There are 2 steps to sort the array in non-decreasing order. Therefore, we return 2.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** 0

**Explanation:** The array is already in non-decreasing order. Therefore, we return 0.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumReplacement(nums: IntArray): Long {
        val n = nums.size
        var prev = nums[n - 1]
        var ans: Long = 0
        for (i in n - 2 downTo 0) {
            var noOfTime = nums[i] / prev
            if (nums[i] % prev != 0) {
                noOfTime++
                prev = nums[i] / noOfTime
            }
            ans += (noOfTime - 1).toLong()
        }
        return ans
    }
}
```