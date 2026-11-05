[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3988\. Create Grid With Exactly K Paths I

Medium

You are given three integers `m`, `n`, and `k`.

Construct **any** `m x n` grid consisting only of the characters `'.'` and `'#'`, where:

*   `'.'` represents a free cell.
*   `'#'` represents an obstacle cell.

A **valid path** is a sequence of free cells that:

*   Starts at the top-left cell `(0, 0)`.
*   Ends at the bottom-right cell `(m - 1, n - 1)`.
*   Moves only:
    *   Right, from `(i, j)` to `(i, j + 1)`, or
    *   Down, from `(i, j)` to `(i + 1, j)`.

Return any grid such that there are **exactly** `k` **valid paths** from the top-left cell to the bottom-right cell. If no such grid exists, return an empty array.

**Example 1:**

**Input:** m = 2, n = 3, k = 2

**Output:** ["...","#.."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-27-at-113554am.png)

There are exactly `k = 2` valid paths from `(0, 0)` to `(1, 2)`:

*   `(0, 0) → (0, 1) → (0, 2) → (1, 2)`
*   `(0, 0) → (0, 1) → (1, 1) → (1, 2)`

**Example 2:**

**Input:** m = 3, n = 3, k = 4

**Output:** ["..#","...","#.."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-27-at-113452am.png)

There are exactly `k = 4` valid paths from `(0, 0)` to `(2, 2)`:

*   `(0, 0) → (0, 1) → (1, 1) → (1, 2) → (2, 2)`
*   `(0, 0) → (0, 1) → (1, 1) → (2, 1) → (2, 2)`
*   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (2, 2)`
*   `(0, 0) → (1, 0) → (1, 1) → (2, 1) → (2, 2)`

**Example 3:**

**Input:** m = 1, n = 4, k = 2

**Output:** []

**Explanation:**

No grid exists with exactly `k = 2` valid paths for a `1 x 4` grid, so the answer is an empty array.

**Constraints:**

*   `1 <= m, n <= 10`
*   `1 <= k <= 4`

## Solution

```kotlin
class Solution {
    fun createGrid(m: Int, n: Int, k: Int): Array<String> {
        val dimensions = getDimensions(m, n, k)
        if (dimensions.isEmpty()) {
            return arrayOf<String>()
        }
        val dm = dimensions[0]
        val dn = dimensions[1]
        val grid = initializeGrid(m, n)
        createRequiredPaths(grid, dm, dn)
        addObstacles(grid, dm, dn, k)
        connectRemainingGrid(grid, m, n, dm, dn)
        return convertToStrings(grid, m)
    }

    private fun getDimensions(m: Int, n: Int, k: Int): IntArray {
        if (k == 1) {
            return intArrayOf(0, 0)
        }
        if (k == 2 && m >= 2 && n >= 2) {
            return intArrayOf(1, 1)
        }
        if (k == 3) {
            if (m >= 3 && n >= 2) {
                return intArrayOf(2, 1)
            }
            if (m >= 2 && n >= 3) {
                return intArrayOf(1, 2)
            }
        }
        if (k == 4) {
            return getFourPathDimensions(m, n)
        }
        return IntArray(0)
    }

    private fun getFourPathDimensions(m: Int, n: Int): IntArray {
        if (m >= 4 && n >= 2) {
            return intArrayOf(3, 1)
        }
        if (m >= 2 && n >= 4) {
            return intArrayOf(1, 3)
        }
        if (m >= 3 && n >= 3) {
            return intArrayOf(2, 2)
        }
        return IntArray(0)
    }

    private fun initializeGrid(m: Int, n: Int): Array<CharArray> {
        val grid: Array<CharArray> = Array<CharArray>(m) { CharArray(n) }
        for (row in grid) {
            row.fill('#')
        }
        return grid
    }

    private fun createRequiredPaths(grid: Array<CharArray>, dm: Int, dn: Int) {
        for (i in 0..dm) {
            for (j in 0..dn) {
                grid[i][j] = '.'
            }
        }
    }

    private fun addObstacles(grid: Array<CharArray>, dm: Int, dn: Int, k: Int) {
        if (dm == 2 && dn == 2 && k == 4) {
            grid[0][2] = '#'
            grid[2][0] = '#'
        }
    }

    private fun connectRemainingGrid(grid: Array<CharArray>, m: Int, n: Int, dm: Int, dn: Int) {
        connectVertically(grid, dm, dn, m)
        connectHorizontally(grid, m, dn, n)
    }

    private fun connectVertically(grid: Array<CharArray>, dm: Int, dn: Int, m: Int) {
        for (i in dm + 1..<m) {
            grid[i][dn] = '.'
        }
    }

    private fun connectHorizontally(grid: Array<CharArray>, m: Int, dn: Int, n: Int) {
        for (j in dn + 1..<n) {
            grid[m - 1][j] = '.'
        }
    }

    private fun convertToStrings(grid: Array<CharArray>, m: Int): Array<String> {
        val ans = Array<String>(m) { "" }
        for (i in 0..<m) {
            ans[i] = String(grid[i])
        }
        return ans
    }
}
```