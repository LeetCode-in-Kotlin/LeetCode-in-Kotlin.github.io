[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 892\. Surface Area of 3D Shapes

Easy

You are given an `n x n` `grid` where you have placed some `1 x 1 x 1` cubes. Each value `v = grid[i][j]` represents a tower of `v` cubes placed on top of cell `(i, j)`.

After placing these cubes, you have decided to glue any directly adjacent cubes to each other, forming several irregular 3D shapes.

Return _the total surface area of the resulting shapes_.

**Note:** The bottom face of each shape counts toward its surface area.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/08/tmp-grid2.jpg)

**Input:** grid = \[\[1,2],[3,4]]

**Output:** 34

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/08/tmp-grid4.jpg)

**Input:** grid = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** 32

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/08/tmp-grid5.jpg)

**Input:** grid = \[\[2,2,2],[2,1,2],[2,2,2]]

**Output:** 46

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 50`
*   `0 <= grid[i][j] <= 50`

## Solution

```kotlin
class Solution {
    fun surfaceArea(grid: Array<IntArray>): Int {
        var surfaceArea = 0
        for (i in grid.indices) {
            for (j in grid[i].indices) {
                if (grid[i][j] > 0) {
                    surfaceArea += 4 * grid[i][j] + 2
                    surfaceArea -= hiddenSides(i, j, grid)
                }
            }
        }
        return surfaceArea
    }

    private fun hiddenSides(i: Int, j: Int, grid: Array<IntArray>): Int {
        var hidden = 0
        val tower = grid[i][j]
        if (j + 1 < grid[i].size && grid[i][j + 1] > 0) {
            hidden += tower.coerceAtMost(grid[i][j + 1])
        }
        if (j - 1 >= 0 && grid[i][j - 1] > 0) {
            hidden += tower.coerceAtMost(grid[i][j - 1])
        }
        if (i + 1 < grid.size && grid[i + 1][j] > 0) {
            hidden += tower.coerceAtMost(grid[i + 1][j])
        }
        if (i - 1 >= 0 && grid[i - 1][j] > 0) {
            hidden += tower.coerceAtMost(grid[i - 1][j])
        }
        return hidden
    }
}
```