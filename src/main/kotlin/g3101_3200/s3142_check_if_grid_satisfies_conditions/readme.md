[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3142\. Check if Grid Satisfies Conditions

Easy

You are given a 2D matrix `grid` of size `m x n`. You need to check if each cell `grid[i][j]` is:

*   Equal to the cell below it, i.e. `grid[i][j] == grid[i + 1][j]` (if it exists).
*   Different from the cell to its right, i.e. `grid[i][j] != grid[i][j + 1]` (if it exists).

Return `true` if **all** the cells satisfy these conditions, otherwise, return `false`.

**Example 1:**

**Input:** grid = \[\[1,0,2],[1,0,2]]

**Output:** true

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)**

All the cells in the grid satisfy the conditions.

**Example 2:**

**Input:** grid = \[\[1,1,1],[0,0,0]]

**Output:** false

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/03/27/example21.png)**

All cells in the first row are equal.

**Example 3:**

**Input:** grid = \[\[1],[2],[3]]

**Output:** false

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

Cells in the first column have different values.

**Constraints:**

*   `1 <= n, m <= 10`
*   `0 <= grid[i][j] <= 9`

## Solution

```kotlin
class Solution {
    fun satisfiesConditions(grid: Array<IntArray>): Boolean {
        val m = grid.size
        val n = grid[0].size
        for (i in 0 until m - 1) {
            if (n > 1) {
                for (j in 0 until n - 1) {
                    if ((grid[i][j] != grid[i + 1][j]) || (grid[i][j] == grid[i][j + 1])) {
                        return false
                    }
                }
            } else {
                if (grid[i][0] != grid[i + 1][0]) {
                    return false
                }
            }
        }
        for (j in 0 until n - 1) {
            if (grid[m - 1][j] == grid[m - 1][j + 1]) {
                return false
            }
        }
        return true
    }
}
```