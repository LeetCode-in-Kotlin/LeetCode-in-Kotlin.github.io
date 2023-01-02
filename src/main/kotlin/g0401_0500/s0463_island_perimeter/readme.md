[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 463\. Island Perimeter

Easy

You are given `row x col` `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.

Grid cells are connected **horizontally/vertically** (not diagonally). The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/island.png)

**Input:** grid = \[\[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]

**Output:** 16

**Explanation:** The perimeter is the 16 yellow stripes in the image above.

**Example 2:**

**Input:** grid = \[\[1]]

**Output:** 4

**Example 3:**

**Input:** grid = \[\[1,0]]

**Output:** 4

**Constraints:**

*   `row == grid.length`
*   `col == grid[i].length`
*   `1 <= row, col <= 100`
*   `grid[i][j]` is `0` or `1`.
*   There is exactly one island in `grid`.

## Solution

```kotlin
class Solution {
    fun islandPerimeter(grid: Array<IntArray>): Int {
        var islands = 0
        var neighbours = 0
        for (i in grid.indices) {
            for (j in grid[i].indices) {
                if (grid[i][j] == 1) {
                    islands++
                    if (i < grid.size - 1 && grid[i + 1][j] == 1) {
                        neighbours++
                    }
                    if (j < grid[i].size - 1 && grid[i][j + 1] == 1) {
                        neighbours++
                    }
                }
            }
        }
        return 4 * islands - 2 * neighbours
    }
}
```