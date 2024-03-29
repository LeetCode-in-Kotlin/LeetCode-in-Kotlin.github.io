[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2536\. Increment Submatrices by One

Medium

You are given a positive integer `n`, indicating that we initially have an `n x n` **0-indexed** integer matrix `mat` filled with zeroes.

You are also given a 2D integer array `query`. For each <code>query[i] = [row1<sub>i</sub>, col1<sub>i</sub>, row2<sub>i</sub>, col2<sub>i</sub>]</code>, you should do the following operation:

*   Add `1` to **every element** in the submatrix with the **top left** corner <code>(row1<sub>i</sub>, col1<sub>i</sub>)</code> and the **bottom right** corner <code>(row2<sub>i</sub>, col2<sub>i</sub>)</code>. That is, add `1` to `mat[x][y]` for all <code>row1<sub>i</sub> <= x <= row2<sub>i</sub></code> and <code>col1<sub>i</sub> <= y <= col2<sub>i</sub></code>.

Return _the matrix_ `mat` _after performing every query._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/11/24/p2example11.png)

**Input:** n = 3, queries = \[\[1,1,2,2],[0,0,1,1]]

**Output:** [[1,1,0],[1,2,1],[0,1,1]]

**Explanation:** The diagram above shows the initial matrix, the matrix after the first query, and the matrix after the second query. 

- In the first query, we add 1 to every element in the submatrix with the top left corner (1, 1) and bottom right corner (2, 2). 

- In the second query, we add 1 to every element in the submatrix with the top left corner (0, 0) and bottom right corner (1, 1).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/11/24/p2example22.png)

**Input:** n = 2, queries = \[\[0,0,1,1]]

**Output:** [[1,1],[1,1]]

**Explanation:** The diagram above shows the initial matrix and the matrix after the first query. 

- In the first query we add 1 to every element in the matrix.

**Constraints:**

*   `1 <= n <= 500`
*   <code>1 <= queries.length <= 10<sup>4</sup></code>
*   <code>0 <= row1<sub>i</sub> <= row2<sub>i</sub> < n</code>
*   <code>0 <= col1<sub>i</sub> <= col2<sub>i</sub> < n</code>

## Solution

```kotlin
class Solution {
    fun rangeAddQueries(n: Int, queries: Array<IntArray>): Array<IntArray> {
        val g = Array(n) { IntArray(n) }
        for (q in queries) {
            val r1 = q[0]
            val c1 = q[1]
            val r2 = q[2]
            val c2 = q[3]
            g[r1][c1]++
            if (c2 < n - 1) {
                g[r1][c2 + 1]--
            }
            if (r2 < n - 1) {
                g[r2 + 1][c1]--
            }
            if (c2 < n - 1 && r2 < n - 1) {
                g[r2 + 1][c2 + 1]++
            }
        }
        for (i in 0 until n) {
            for (j in 0 until n) {
                if (i > 0) {
                    g[i][j] += g[i - 1][j]
                }
                if (j > 0) {
                    g[i][j] += g[i][j - 1]
                }
                if (i > 0 && j > 0) {
                    g[i][j] -= g[i - 1][j - 1]
                }
            }
        }
        return g
    }
}
```