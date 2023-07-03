[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2352\. Equal Row and Column Pairs

Medium

Given a **0-indexed** `n x n` integer matrix `grid`, _return the number of pairs_ <code>(R<sub>i</sub>, C<sub>j</sub>)</code> _such that row_ <code>R<sub>i</sub></code> _and column_ <code>C<sub>j</sub></code> _are equal_.

A row and column pair is considered equal if they contain the same elements in the same order (i.e. an equal array).

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg)

**Input:** grid = \[\[3,2,1],[1,7,6],[2,7,7]]

**Output:** 1

**Explanation:** There is 1 equal row and column pair:

- (Row 2, Column 1): [2,7,7] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg)

**Input:** grid = \[\[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]

**Output:** 3

**Explanation:** There are 3 equal row and column pairs:

- (Row 0, Column 0): [3,1,2,2]

- (Row 2, Column 2): [2,4,2,2]

- (Row 3, Column 2): [2,4,2,2]

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 200`
*   <code>1 <= grid[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun equalPairs(grid: Array<IntArray>): Int {
        val rows: MutableMap<Int, Int> = HashMap()
        for (i in grid.indices) {
            val hash = getRowHash(grid[i])
            rows[hash] = rows.getOrDefault(hash, 0) + 1
        }
        var count = 0
        for (i in grid.indices) {
            val hash = getColHash(grid, i)
            count += rows.getOrDefault(hash, 0)
        }
        return count
    }

    private fun getRowHash(grid: IntArray): Int {
        var res = 11
        for (i in grid) res = res * 11 + i
        return res
    }

    private fun getColHash(grid: Array<IntArray>, index: Int): Int {
        var res = 11
        for (i in grid.indices) {
            res = res * 11 + grid[i][index]
        }
        return res
    }
}
```