[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2639\. Find the Width of Columns of a Grid

Easy

You are given a **0-indexed** `m x n` integer matrix `grid`. The width of a column is the maximum **length** of its integers.

*   For example, if `grid = \[\[-10], [3], [12]]`, the width of the only column is `3` since `-10` is of length `3`.

Return _an integer array_ `ans` _of size_ `n` _where_ `ans[i]` _is the width of the_ <code>i<sup>th</sup></code> _column_.

The **length** of an integer `x` with `len` digits is equal to `len` if `x` is non-negative, and `len + 1` otherwise.

**Example 1:**

**Input:** grid = \[\[1],[22],[333]]

**Output:** [3]

**Explanation:** In the 0<sup>th</sup> column, 333 is of length 3.

**Example 2:**

**Input:** grid = \[\[-15,1,3],[15,7,12],[5,6,-2]]

**Output:** [3,1,2]

**Explanation:** 

In the 0<sup>th</sup> column, only -15 is of length 3. 

In the 1<sup>st</sup> column, all integers are of length 1. 

In the 2<sup>nd</sup> column, both 12 and -2 are of length 2.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 100`
*   <code>-10<sup>9</sup> <= grid[r][c] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun findColumnWidth(grid: Array<IntArray>): IntArray {
        val ans = IntArray(grid[0].size) { 1 }
        for (j in 0 until grid[0].size) {
            var max = 1
            for (i in 0 until grid.size) {
                var num = grid[i][j]
                var neg = false
                if (num < 0) {
                    neg = true
                    num *= -1
                }
                var size = 0
                while (num > 0) {
                    num /= 10
                    size++
                }
                if (neg) {
                    size += 1
                }
                max = maxOf(max, size)
            }
            ans[j] = max
        }
        return ans
    }
}
```