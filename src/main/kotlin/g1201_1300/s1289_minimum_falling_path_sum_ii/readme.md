[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1289\. Minimum Falling Path Sum II

Hard

Given an `n x n` integer matrix `grid`, return _the minimum sum of a **falling path with non-zero shifts**_.

A **falling path with non-zero shifts** is a choice of exactly one element from each row of `grid` such that no two elements chosen in adjacent rows are in the same column.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg)

**Input:** arr = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** 13

**Explanation:** The possible falling paths are: [1,5,9], [1,5,7], [1,6,7], [1,6,8], [2,4,8], [2,4,9], [2,6,7], [2,6,8], [3,4,8], [3,4,9], [3,5,7], [3,5,9] The falling path with the smallest sum is [1,5,7], so the answer is 13.

**Example 2:**

**Input:** grid = \[\[7]]

**Output:** 7

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 200`
*   `-99 <= grid[i][j] <= 99`

## Solution

```kotlin
class Solution {
    fun minFallingPathSum(grid: Array<IntArray>): Int {
        val n = grid[0].size
        var prev = IntArray(n)
        var curr = IntArray(n)
        var prevMinOne = 0
        var prevMinTwo = 0
        for (ints in grid) {
            var currMinOne = Int.MAX_VALUE
            var currMinTwo = Int.MAX_VALUE
            for (j in 0 until n) {
                val prevMin = if (prev[j] == prevMinOne) prevMinTwo else prevMinOne
                curr[j] = ints[j] + prevMin
                if (curr[j] < currMinOne) {
                    currMinTwo = currMinOne
                    currMinOne = curr[j]
                } else if (curr[j] < currMinTwo) {
                    currMinTwo = curr[j]
                }
            }
            prevMinOne = currMinOne
            prevMinTwo = currMinTwo
            // reuse curr array, avoid new int[] in every row
            val temp = prev
            prev = curr
            curr = temp
        }
        return prevMinOne
    }
}
```