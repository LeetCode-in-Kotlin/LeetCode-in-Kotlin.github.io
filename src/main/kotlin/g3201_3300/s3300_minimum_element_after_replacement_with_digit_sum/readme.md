[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3300\. Minimum Element After Replacement With Digit Sum

Easy

You are given an integer array `nums`.

You replace each element in `nums` with the **sum** of its digits.

Return the **minimum** element in `nums` after all replacements.

**Example 1:**

**Input:** nums = [10,12,13,14]

**Output:** 1

**Explanation:**

`nums` becomes `[1, 3, 4, 5]` after all replacements, with minimum element 1.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 1

**Explanation:**

`nums` becomes `[1, 2, 3, 4]` after all replacements, with minimum element 1.

**Example 3:**

**Input:** nums = [999,19,199]

**Output:** 10

**Explanation:**

`nums` becomes `[27, 10, 19]` after all replacements, with minimum element 10.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minElement(nums: IntArray): Int {
        var min = Int.Companion.MAX_VALUE
        for (x in nums) {
            min = min(min, solve(x))
        }
        return min
    }

    private fun solve(x: Int): Int {
        var x = x
        var sum = 0
        while (x != 0) {
            sum += x % 10
            x /= 10
        }
        return sum
    }
}
```