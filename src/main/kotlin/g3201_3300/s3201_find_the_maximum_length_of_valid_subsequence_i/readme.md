[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3201\. Find the Maximum Length of Valid Subsequence I

Medium

You are given an integer array `nums`.

A subsequence `sub` of `nums` with length `x` is called **valid** if it satisfies:

*   `(sub[0] + sub[1]) % 2 == (sub[1] + sub[2]) % 2 == ... == (sub[x - 2] + sub[x - 1]) % 2.`

Return the length of the **longest** **valid** subsequence of `nums`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [1,2,3,4]

**Output:** 4

**Explanation:**

The longest valid subsequence is `[1, 2, 3, 4]`.

**Example 2:**

**Input:** nums = [1,2,1,1,2,1,2]

**Output:** 6

**Explanation:**

The longest valid subsequence is `[1, 2, 1, 2, 1, 2]`.

**Example 3:**

**Input:** nums = [1,3]

**Output:** 2

**Explanation:**

The longest valid subsequence is `[1, 3]`.

**Constraints:**

*   <code>2 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>7</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLength(nums: IntArray): Int {
        val n = nums.size
        var alter = 1
        var odd = 0
        var even = 0
        if (nums[0] % 2 == 0) {
            even++
        } else {
            odd++
        }
        var lastodd = nums[0] % 2 != 0
        for (i in 1 until n) {
            val flag = nums[i] % 2 == 0
            if (flag) {
                if (lastodd) {
                    alter++
                    lastodd = false
                }
                even++
            } else {
                if (!lastodd) {
                    alter++
                    lastodd = true
                }
                odd++
            }
        }
        return max(alter, max(odd, even))
    }
}
```