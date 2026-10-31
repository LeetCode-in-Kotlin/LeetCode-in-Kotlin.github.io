[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3745\. Maximize Expression of Three Elements

Easy

You are given an integer array `nums`.

Choose three elements `a`, `b`, and `c` from `nums` at **distinct** indices such that the value of the expression `a + b - c` is maximized.

Return an integer denoting the **maximum possible value** of this expression.

**Example 1:**

**Input:** nums = [1,4,2,5]

**Output:** 8

**Explanation:**

We can choose `a = 4`, `b = 5`, and `c = 1`. The expression value is `4 + 5 - 1 = 8`, which is the maximum possible.

**Example 2:**

**Input:** nums = [-2,0,5,-2,4]

**Output:** 11

**Explanation:**

We can choose `a = 5`, `b = 4`, and `c = -2`. The expression value is `5 + 4 - (-2) = 11`, which is the maximum possible.

**Constraints:**

*   `3 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun maximizeExpressionOfThree(nums: IntArray): Int {
        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE
        var min = Int.MAX_VALUE
        for (num in nums) {
            if (num > max1) {
                max2 = max1
                max1 = num
            } else if (num > max2) {
                max2 = num
            }
            if (num < min) {
                min = num
            }
        }
        return max1 + max2 - min
    }
}
```