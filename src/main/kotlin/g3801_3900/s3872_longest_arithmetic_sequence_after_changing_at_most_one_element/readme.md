[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3872\. Longest Arithmetic Sequence After Changing At Most One Element

Medium

You are given an integer array `nums`.

A subarray is **arithmetic** if the difference between consecutive elements in the subarray is constant.

You can replace **at most one** element in `nums` with any **integer**. Then, you select an arithmetic subarray from `nums`.

Return an integer denoting the **maximum** length of the arithmetic subarray you can select.

**Example 1:**

**Input:** nums = [9,7,5,10,1]

**Output:** 5

**Explanation:**

*   Replace `nums[3] = 10` with 3. The array becomes `[9, 7, 5, 3, 1]`.
*   Select the subarray <code>[<ins>**9, 7, 5, 3, 1**</ins>]</code>, which is arithmetic because consecutive elements have a common difference of -2.

**Example 2:**

**Input:** nums = [1,2,6,7]

**Output:** 3

**Explanation:**

*   Replace `nums[0] = 1` with -2. The array becomes `[-2, 2, 6, 7]`.
*   Select the subarray <code>[<ins>**-2, 2, 6**</ins>, 7]</code>, which is arithmetic because consecutive elements have a common difference of 4.

**Constraints:**

*   <code>4 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun longestArithmetic(nums: IntArray): Int {
        val ans: Int = solve(nums)
        reverse(nums)
        return max(ans, solve(nums))
    }

    companion object {
        private fun solve(nums: IntArray): Int {
            val n = nums.size
            var max = 2
            var diff = nums[1] - nums[0]
            var left = 0
            for (right in 2..<n) {
                if (nums[right] - nums[right - 1] == diff) {
                    max = max(max, right - left + 1)
                    continue
                }
                var temp = right
                var `val` = nums[right - 1] + diff
                while (temp + 1 < n && nums[temp + 1] - `val` == diff) {
                    `val` = nums[++temp]
                }
                max = max(max, temp - left + 1)
                left = right - 1
                diff = nums[right] - nums[right - 1]
            }
            return max
        }

        private fun reverse(nums: IntArray) {
            var i = 0
            var j = nums.size - 1
            while (i < j) {
                val temp = nums[i]
                nums[i++] = nums[j]
                nums[j--] = temp
            }
        }
    }
}
```