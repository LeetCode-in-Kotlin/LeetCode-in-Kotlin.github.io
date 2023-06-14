[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1536\. Minimum Swaps to Arrange a Binary Grid

Medium

Given an `n x n` binary `grid`, in one step you can choose two **adjacent rows** of the grid and swap them.

A grid is said to be **valid** if all the cells above the main diagonal are **zeros**.

Return _the minimum number of steps_ needed to make the grid valid, or **\-1** if the grid cannot be valid.

The main diagonal of a grid is the diagonal that starts at cell `(1, 1)` and ends at cell `(n, n)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/28/fw.jpg)

**Input:** grid = \[\[0,0,1],[1,1,0],[1,0,0]]

**Output:** 3

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/07/16/e2.jpg)

**Input:** grid = \[\[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]

**Output:** -1

**Explanation:** All rows are similar, swaps have no effect on the grid.

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/07/16/e3.jpg)

**Input:** grid = \[\[1,0,0],[1,1,0],[1,1,1]]

**Output:** 0

**Constraints:**

*   `n == grid.length` `== grid[i].length`
*   `1 <= n <= 200`
*   `grid[i][j]` is either `0` or `1`

## Solution

```kotlin
class Solution {
    fun minSwaps(grid: Array<IntArray>): Int {
        val len = grid.size
        var swap = 0
        val preProcess = IntArray(len)
        for (i in 0 until len) {
            preProcess[i] = countRightZeros(grid[i])
        }
        for (i in 0 until len) {
            val minValueRequired = len - i - 1
            var j = i
            while (j < len && preProcess[j] < minValueRequired) {
                j++
            }
            if (j == len) {
                return -1
            }
            while (j != i) {
                swap++
                val temp = preProcess[j]
                preProcess[j] = preProcess[j - 1]
                preProcess[j - 1] = temp
                j--
            }
        }
        return swap
    }

    private fun countRightZeros(row: IntArray): Int {
        var cnt = 0
        for (i in row.indices.reversed()) {
            if (row[i] != 0) {
                break
            }
            cnt++
        }
        return cnt
    }
}
```