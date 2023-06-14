[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1591\. Strange Printer II

Hard

There is a strange printer with the following two special requirements:

*   On each turn, the printer will print a solid rectangular pattern of a single color on the grid. This will cover up the existing colors in the rectangle.
*   Once the printer has used a color for the above operation, **the same color cannot be used again**.

You are given a `m x n` matrix `targetGrid`, where `targetGrid[row][col]` is the color in the position `(row, col)` of the grid.

Return `true` _if it is possible to print the matrix_ `targetGrid`_,_ _otherwise, return_ `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/23/print1.jpg)

**Input:** targetGrid = \[\[1,1,1,1],[1,2,2,1],[1,2,2,1],[1,1,1,1]]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/23/print2.jpg)

**Input:** targetGrid = \[\[1,1,1,1],[1,1,3,3],[1,1,3,4],[5,5,1,4]]

**Output:** true

**Example 3:**

**Input:** targetGrid = \[\[1,2,1],[2,1,2],[1,2,1]]

**Output:** false

**Explanation:** It is impossible to form targetGrid because it is not allowed to print the same color in different turns.

**Constraints:**

*   `m == targetGrid.length`
*   `n == targetGrid[i].length`
*   `1 <= m, n <= 60`
*   `1 <= targetGrid[row][col] <= 60`

## Solution

```kotlin
class Solution {
    fun isPrintable(targetGrid: Array<IntArray>): Boolean {
        val colorBound = Array(61) { IntArray(4) }
        val colors: MutableSet<Int> = HashSet()
        // prepare colorBound with Max and Min integer for later compare
        for (i in colorBound.indices) {
            for (j in colorBound[0].indices) {
                if (j == 0 || j == 1) {
                    colorBound[i][j] = Int.MAX_VALUE
                } else {
                    colorBound[i][j] = Int.MIN_VALUE
                }
            }
        }
        // find the color range for each color
        // each color i has a colorBound[i] with {min_i, min_j, max_i, max_j}
        for (i in targetGrid.indices) {
            for (j in targetGrid[0].indices) {
                colorBound[targetGrid[i][j]][0] = Math.min(colorBound[targetGrid[i][j]][0], i)
                colorBound[targetGrid[i][j]][1] = Math.min(colorBound[targetGrid[i][j]][1], j)
                colorBound[targetGrid[i][j]][2] = Math.max(colorBound[targetGrid[i][j]][2], i)
                colorBound[targetGrid[i][j]][3] = Math.max(colorBound[targetGrid[i][j]][3], j)
                colors.add(targetGrid[i][j])
            }
        }
        val printed = BooleanArray(61)
        val visited = Array(targetGrid.size) { BooleanArray(targetGrid[0].size) }
        // DFS all the colors, skip the color already be printed
        for (color in colors) {
            if (printed[color]) {
                continue
            }
            if (!dfs(targetGrid, printed, colorBound, visited, color)) {
                return false
            }
        }
        // if all color has been printed, then return true
        return true
    }

    private fun dfs(
        targetGrid: Array<IntArray>,
        printed: BooleanArray,
        colorBound: Array<IntArray>,
        visited: Array<BooleanArray>,
        color: Int
    ): Boolean {
        printed[color] = true
        for (i in colorBound[color][0]..colorBound[color][2]) {
            for (j in colorBound[color][1]..colorBound[color][3]) {
                // if i, j is already visited, skip
                if (visited[i][j]) {
                    continue
                }
                // if we find a different color, then check if the color is already printed, if so,
                // return false
                // otherwise, dfs the range of the new color
                if (targetGrid[i][j] != color) {
                    if (printed[targetGrid[i][j]]) {
                        return false
                    }
                    if (!dfs(targetGrid, printed, colorBound, visited, targetGrid[i][j])) {
                        return false
                    }
                }
                visited[i][j] = true
            }
        }
        return true
    }
}
```