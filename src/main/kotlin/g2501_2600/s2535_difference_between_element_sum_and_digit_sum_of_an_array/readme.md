[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2535\. Difference Between Element Sum and Digit Sum of an Array

Easy

You are given a positive integer array `nums`.

*   The **element sum** is the sum of all the elements in `nums`.
*   The **digit sum** is the sum of all the digits (not necessarily distinct) that appear in `nums`.

Return _the **absolute** difference between the **element sum** and **digit sum** of_ `nums`.

**Note** that the absolute difference between two integers `x` and `y` is defined as `|x - y|`.

**Example 1:**

**Input:** nums = [1,15,6,3]

**Output:** 9

**Explanation:** 

The element sum of nums is 1 + 15 + 6 + 3 = 25. 

The digit sum of nums is 1 + 1 + 5 + 6 + 3 = 16. 

The absolute difference between the element sum and digit sum is \|25 - 16\| = 9.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 0

**Explanation:** 

The element sum of nums is 1 + 2 + 3 + 4 = 10.

The digit sum of nums is 1 + 2 + 3 + 4 = 10.

The absolute difference between the element sum and digit sum is \|10 - 10\| = 0.

**Constraints:**

*   `1 <= nums.length <= 2000`
*   `1 <= nums[i] <= 2000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun getsum(n: Int): Int {
        var n = n
        var sum = 0
        while (n > 0) {
            val r = n % 10
            sum += r
            n = n / 10
        }
        return sum
    }

    fun differenceOfSum(nums: IntArray): Int {
        var eleSum = 0
        var digitSum = 0
        for (j in nums) {
            digitSum += if (j >= 10) {
                val sumC = getsum(j)
                sumC
            } else {
                j
            }
        }
        for (num in nums) {
            eleSum += num
        }
        val max = Math.max(eleSum, digitSum)
        val min = Math.min(eleSum, digitSum)
        return max - min
    }
}
```