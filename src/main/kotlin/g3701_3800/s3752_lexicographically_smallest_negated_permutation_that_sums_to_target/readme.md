[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3752\. Lexicographically Smallest Negated Permutation that Sums to Target

Medium

You are given a positive integer `n` and an integer `target`.

Return the **lexicographically smallest** array of integers of size `n` such that:

*   The **sum** of its elements equals `target`.
*   The **absolute values** of its elements form a **permutation** of size `n`.

If no such array exists, return an empty array.

A **permutation** of size `n` is a rearrangement of integers `1, 2, ..., n`.

**Example 1:**

**Input:** n = 3, target = 0

**Output:** [-3,1,2]

**Explanation:**

The arrays that sum to 0 and whose absolute values form a permutation of size 3 are:

*   `[-3, 1, 2]`
*   `[-3, 2, 1]`
*   `[-2, -1, 3]`
*   `[-2, 3, -1]`
*   `[-1, -2, 3]`
*   `[-1, 3, -2]`
*   `[1, -3, 2]`
*   `[1, 2, -3]`
*   `[2, -3, 1]`
*   `[2, 1, -3]`
*   `[3, -2, -1]`
*   `[3, -1, -2]`

The lexicographically smallest one is `[-3, 1, 2]`.

**Example 2:**

**Input:** n = 1, target = 10000000000

**Output:** []

**Explanation:**

There are no arrays that sum to 10000000000 and whose absolute values form a permutation of size 1. Therefore, the answer is `[]`.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>10</sup> <= target <= 10<sup>10</sup></code>

## Solution

```kotlin
class Solution {
    fun lexSmallestNegatedPerm(n: Int, target: Long): IntArray? {
        var drop = n.toLong() * (n + 1) / 2 - target
        if (drop < 0 || drop % 2 != 0L) {
            return intArrayOf()
        }
        val ans = IntArray(n)
        var l = 0
        var r = n - 1
        for (i in n downTo 1) {
            val `val` = i
            if (2 * `val` <= drop) {
                drop -= (2 * `val`).toLong()
                ans[l++] = -`val`
            } else {
                ans[r--] = `val`
            }
        }
        if (drop != 0L) {
            return intArrayOf()
        }
        return ans
    }
}
```