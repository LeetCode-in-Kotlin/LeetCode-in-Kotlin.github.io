[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2397\. Maximum Rows Covered by Columns

Medium

You are given a **0-indexed** `m x n` binary matrix `matrix` and an integer `numSelect`, which denotes the number of **distinct** columns you must select from `matrix`.

Let us consider <code>s = {c<sub>1</sub>, c<sub>2</sub>, ...., c<sub>numSelect</sub>}</code> as the set of columns selected by you. A row `row` is **covered** by `s` if:

*   For each cell `matrix[row][col]` (`0 <= col <= n - 1`) where `matrix[row][col] == 1`, `col` is present in `s` or,
*   **No cell** in `row` has a value of `1`.

You need to choose `numSelect` columns such that the number of rows that are covered is **maximized**.

Return _the **maximum** number of rows that can be **covered** by a set of_ `numSelect` _columns._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/07/14/rowscovered.png)

**Input:** matrix = \[\[0,0,0],[1,0,1],[0,1,1],[0,0,1]], numSelect = 2

**Output:** 3

**Explanation:** One possible way to cover 3 rows is shown in the diagram above.

We choose s = {0, 2}.

- Row 0 is covered because it has no occurrences of 1.

- Row 1 is covered because the columns with value 1, i.e. 0 and 2 are present in s.

- Row 2 is not covered because matrix[2][1] == 1 but 1 is not present in s.

- Row 3 is covered because matrix[2][2] == 1 and 2 is present in s.

Thus, we can cover three rows.

Note that s = {1, 2} will also cover 3 rows, but it can be shown that no more than three rows can be covered. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/07/14/rowscovered2.png)

**Input:** matrix = \[\[1],[0]], numSelect = 1

**Output:** 2

**Explanation:** Selecting the only column will result in both rows being covered since the entire matrix is selected.

Therefore, we return 2. 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 12`
*   `matrix[i][j]` is either `0` or `1`.
*   `1 <= numSelect <= n`

## Solution

```kotlin
class Solution {
    private var ans = 0
    fun maximumRows(matrix: Array<IntArray>, numSelect: Int): Int {
        dfs(matrix, /*colIndex=*/0, numSelect, /*mask=*/0)
        return ans
    }

    private fun dfs(matrix: Array<IntArray>, colIndex: Int, leftColsCount: Int, mask: Int) {
        if (leftColsCount == 0) {
            ans = Math.max(ans, getAllZerosRowCount(matrix, mask))
            return
        }
        if (colIndex == matrix[0].size) {
            return
        }
        // choose this column
        dfs(matrix, colIndex + 1, leftColsCount - 1, mask or (1 shl colIndex))
        // not choose this column
        dfs(matrix, colIndex + 1, leftColsCount, mask)
    }

    private fun getAllZerosRowCount(matrix: Array<IntArray>, mask: Int): Int {
        var count = 0
        for (row in matrix) {
            var isAllZeros = true
            for (i in row.indices) {
                if (row[i] == 1 && mask shr i and 1 == 0) {
                    isAllZeros = false
                    break
                }
            }
            if (isAllZeros) {
                ++count
            }
        }
        return count
    }
}
```