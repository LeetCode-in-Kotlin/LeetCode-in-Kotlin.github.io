[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3567\. Minimum Absolute Difference in Sliding Submatrix

Medium

You are given an `m x n` integer matrix `grid` and an integer `k`.

For every contiguous `k x k` **submatrix** of `grid`, compute the **minimum absolute** difference between any two **distinct** values within that **submatrix**.

Return a 2D array `ans` of size `(m - k + 1) x (n - k + 1)`, where `ans[i][j]` is the minimum absolute difference in the submatrix whose top-left corner is `(i, j)` in `grid`.

**Note**: If all elements in the submatrix have the same value, the answer will be 0.

A submatrix `(x1, y1, x2, y2)` is a matrix that is formed by choosing all cells `matrix[x][y]` where `x1 <= x <= x2` and `y1 <= y <= y2`.

**Example 1:**

**Input:** grid = \[\[1,8],[3,-2]], k = 2

**Output:** [[2]]

**Explanation:**

*   There is only one possible `k x k` submatrix: `[[1, 8], [3, -2]]`.
*   Distinct values in the submatrix are `[1, 8, 3, -2]`.
*   The minimum absolute difference in the submatrix is `|1 - 3| = 2`. Thus, the answer is `[[2]]`.

**Example 2:**

**Input:** grid = \[\[3,-1]], k = 1

**Output:** [[0,0]]

**Explanation:**

*   Both `k x k` submatrix has only one distinct element.
*   Thus, the answer is `[[0, 0]]`.

**Example 3:**

**Input:** grid = \[\[1,-2,3],[2,3,5]], k = 2

**Output:** [[1,2]]

**Explanation:**

*   There are two possible `k × k` submatrix:
    *   Starting at `(0, 0)`: `[[1, -2], [2, 3]]`.
        *   Distinct values in the submatrix are `[1, -2, 2, 3]`.
        *   The minimum absolute difference in the submatrix is `|1 - 2| = 1`.
    *   Starting at `(0, 1)`: `[[-2, 3], [3, 5]]`.
        *   Distinct values in the submatrix are `[-2, 3, 5]`.
        *   The minimum absolute difference in the submatrix is `|3 - 5| = 2`.
*   Thus, the answer is `[[1, 2]]`.

**Constraints:**

*   `1 <= m == grid.length <= 30`
*   `1 <= n == grid[i].length <= 30`
*   <code>-10<sup>5</sup> <= grid[i][j] <= 10<sup>5</sup></code>
*   `1 <= k <= min(m, n)`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minAbsDiff(grid: Array<IntArray>, k: Int): Array<IntArray> {
        val rows = grid.size
        val cols = grid[0].size
        val result = Array<IntArray>(rows - k + 1) { IntArray(cols - k + 1) }
        for (x in 0..rows - k) {
            for (y in 0..cols - k) {
                val size = k * k
                val elements = IntArray(size)
                var idx = 0
                for (i in x..<x + k) {
                    for (j in y..<y + k) {
                        elements[idx++] = grid[i][j]
                    }
                }
                elements.sort()
                var minDiff = Int.Companion.MAX_VALUE
                for (i in 1..<size) {
                    if (elements[i] != elements[i - 1]) {
                        minDiff = min(minDiff, elements[i] - elements[i - 1])
                        if (minDiff == 1) {
                            break
                        }
                    }
                }
                result[x][y] = if (minDiff == Int.Companion.MAX_VALUE) 0 else minDiff
            }
        }
        return result
    }
}
```