[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3127\. Make a Square with the Same Color

Easy

You are given a 2D matrix `grid` of size `3 x 3` consisting only of characters `'B'` and `'W'`. Character `'W'` represents the white color, and character `'B'` represents the black color.

Your task is to change the color of **at most one** cell so that the matrix has a `2 x 2` square where all cells are of the same color.

Return `true` if it is possible to create a `2 x 2` square of the same color, otherwise, return `false`.

**Example 1:**

**Input:** grid = \[\["B","W","B"],["B","W","W"],["B","W","B"]]

**Output:** true

**Explanation:**

It can be done by changing the color of the `grid[0][2]`.

**Example 2:**

**Input:** grid = \[\["B","W","B"],["W","B","W"],["B","W","B"]]

**Output:** false

**Explanation:**

It cannot be done by changing at most one cell.

**Example 3:**

**Input:** grid = \[\["B","W","B"],["B","W","W"],["B","W","W"]]

**Output:** true

**Explanation:**

The `grid` already contains a `2 x 2` square of the same color.

**Constraints:**

*   `grid.length == 3`
*   `grid[i].length == 3`
*   `grid[i][j]` is either `'W'` or `'B'`.

## Solution

```kotlin
class Solution {
    fun canMakeSquare(grid: Array<CharArray>): Boolean {
        val n = grid.size
        val m = grid[0].size
        for (i in 0 until n - 1) {
            for (j in 0 until m - 1) {
                var countBlack = 0
                var countWhite = 0
                for (k in i..i + 1) {
                    for (l in j..j + 1) {
                        if (grid[k][l] == 'W') {
                            countWhite++
                        } else {
                            countBlack++
                        }
                    }
                }
                if (countBlack >= 3 || countWhite >= 3) {
                    return true
                }
            }
        }
        return false
    }
}
```