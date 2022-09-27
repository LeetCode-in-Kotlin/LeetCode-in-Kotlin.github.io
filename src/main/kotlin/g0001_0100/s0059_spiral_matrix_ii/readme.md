[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 59\. Spiral Matrix II

Medium

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to <code>n<sup>2</sup></code> in spiral order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

**Input:** n = 3

**Output:** [[1,2,3],[8,9,4],[7,6,5]]

**Example 2:**

**Input:** n = 1

**Output:** [[1]]

**Constraints:**

*   `1 <= n <= 20`

## Solution

```kotlin
class Solution {
    fun generateMatrix(n: Int): Array<IntArray> {
        var num = 1
        var rStart = 0
        var rEnd = n - 1
        var cStart = 0
        var cEnd = n - 1
        val spiral = Array(n) { IntArray(n) }
        while (rStart <= rEnd && cStart <= cEnd) {
            for (k in cStart..cEnd) {
                spiral[rStart][k] = num++
            }
            rStart++
            for (k in rStart..rEnd) {
                spiral[k][cEnd] = num++
            }
            cEnd--
            if (rStart <= rEnd) {
                for (k in cEnd downTo cStart) {
                    spiral[rEnd][k] = num++
                }
            }
            rEnd--
            if (cStart <= cEnd) {
                for (k in rEnd downTo rStart) {
                    spiral[k][cStart] = num++
                }
            }
            cStart++
        }
        return spiral
    }
}
```