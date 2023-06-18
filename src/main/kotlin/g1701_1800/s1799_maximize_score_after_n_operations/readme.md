[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1799\. Maximize Score After N Operations

Hard

You are given `nums`, an array of positive integers of size `2 * n`. You must perform `n` operations on this array.

In the <code>i<sup>th</sup></code> operation **(1-indexed)**, you will:

*   Choose two elements, `x` and `y`.
*   Receive a score of `i * gcd(x, y)`.
*   Remove `x` and `y` from `nums`.

Return _the maximum score you can receive after performing_ `n` _operations._

The function `gcd(x, y)` is the greatest common divisor of `x` and `y`.

**Example 1:**

**Input:** nums = [1,2]

**Output:** 1

**Explanation:** The optimal choice of operations is:

v(1 \* gcd(1, 2)) = 1

**Example 2:**

**Input:** nums = [3,4,6,8]

**Output:** 11

**Explanation:** The optimal choice of operations is:

(1 \* gcd(3, 6)) + (2 \* gcd(4, 8)) = 3 + 8 = 11

**Example 3:**

**Input:** nums = [1,2,3,4,5,6]

**Output:** 14

**Explanation:** The optimal choice of operations is:

(1 \* gcd(1, 5)) + (2 \* gcd(2, 4)) + (3 \* gcd(3, 6)) = 1 + 4 + 9 = 14

**Constraints:**

*   `1 <= n <= 7`
*   `nums.length == 2 * n`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun maxScore(nums: IntArray): Int {
        val n = nums.size
        val memo = arrayOfNulls<Int>(1 shl n)
        return helper(1, 0, nums, memo)
    }

    private fun helper(operationNumber: Int, mask: Int, nums: IntArray, memo: Array<Int?>): Int {
        val n = nums.size
        if (memo[mask] != null) {
            return memo[mask]!!
        }
        if (operationNumber > n / 2) {
            return 0
        }
        var maxScore = Int.MIN_VALUE
        for (i in 0 until n) {
            if (mask and (1 shl i) == 0) {
                for (j in i + 1 until n) {
                    if (mask and (1 shl j) == 0) {
                        val score = operationNumber * gcd(nums[i], nums[j])
                        val score2 = helper(operationNumber + 1, mask or (1 shl i) or (1 shl j), nums, memo)
                        maxScore = Math.max(maxScore, score + score2)
                    }
                }
            }
        }
        memo[mask] = maxScore
        return maxScore
    }

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) {
            a
        } else gcd(b, a % b)
    }
}
```