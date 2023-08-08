[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2732\. Find a Good Subset of the Matrix

Hard

You are given a **0-indexed** `m x n` binary matrix `grid`.

Let us call a **non-empty** subset of rows **good** if the sum of each column of the subset is at most half of the length of the subset.

More formally, if the length of the chosen subset of rows is `k`, then the sum of each column should be at most `floor(k / 2)`.

Return _an integer array that contains row indices of a good subset sorted in **ascending** order._

If there are multiple good subsets, you can return any of them. If there are no good subsets, return an empty array.

A **subset** of rows of the matrix `grid` is any matrix that can be obtained by deleting some (possibly none or all) rows from `grid`.

**Example 1:**

**Input:** grid = \[\[0,1,1,0],[0,0,0,1],[1,1,1,1]]

**Output:** [0,1]

**Explanation:** We can choose the 0<sup>th</sup> and 1<sup>st</sup> rows to create a good subset of rows. The length of the chosen subset is 2. 
- The sum of the 0<sup>th</sup> column is 0 + 0 = 0, which is at most half of the length of the subset. 
- The sum of the 1<sup>st</sup> column is 1 + 0 = 1, which is at most half of the length of the subset. 
- The sum of the 2<sup>nd</sup> column is 1 + 0 = 1, which is at most half of the length of the subset. 
- The sum of the 3<sup>rd</sup> column is 0 + 1 = 1, which is at most half of the length of the subset.

**Example 2:**

**Input:** grid = \[\[0]]

**Output:** [0]

**Explanation:** We can choose the 0<sup>th</sup> row to create a good subset of rows. The length of the chosen subset is 1. 
- The sum of the 0<sup>th</sup> column is 0, which is at most half of the length of the subset.

**Example 3:**

**Input:** grid = \[\[1,1,1],[1,1,1]]

**Output:** []

**Explanation:** It is impossible to choose any subset of rows to create a good subset.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m <= 10<sup>4</sup></code>
*   `1 <= n <= 5`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun goodSubsetofBinaryMatrix(grid: Array<IntArray>): List<Int> {
        val m = grid.size
        val n = grid[0].size
        if (m == 1 && grid[0].sum() == 0) {
            return listOf(0)
        }
        val pos = mutableMapOf<Int, Int>()
        for (i in grid.indices) {
            for (mask in 0 until (1 shl n)) {
                var valid = true
                for (j in 0 until n) {
                    if ((mask and (1 shl j)) != 0 && grid[i][j] + 1 > 1) {
                        valid = false
                        break
                    }
                }
                if (valid && mask in pos) {
                    return listOf(pos[mask]!!, i)
                }
            }
            var curr = 0
            for (j in 0 until n) {
                if (grid[i][j] == 1) {
                    curr = curr or (1 shl j)
                }
            }
            pos[curr] = i
        }
        return emptyList()
    }
}
```