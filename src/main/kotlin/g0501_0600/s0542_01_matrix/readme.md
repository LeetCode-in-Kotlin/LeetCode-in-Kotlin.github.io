[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 542\. 01 Matrix

Medium

Given an `m x n` binary matrix `mat`, return _the distance of the nearest_ `0` _for each cell_.

The distance between two adjacent cells is `1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-1-grid.jpg)

**Input:** mat = \[\[0,0,0],[0,1,0],[0,0,0]]

**Output:** [[0,0,0],[0,1,0],[0,0,0]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-2-grid.jpg)

**Input:** mat = \[\[0,0,0],[0,1,0],[1,1,1]]

**Output:** [[0,0,0],[0,1,0],[1,2,1]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   <code>1 <= m, n <= 10<sup>4</sup></code>
*   <code>1 <= m * n <= 10<sup>4</sup></code>
*   `mat[i][j]` is either `0` or `1`.
*   There is at least one `0` in `mat`.

## Solution

```kotlin
class Solution {
    fun updateMatrix(mat: Array<IntArray>): Array<IntArray> {
        val dist = Array(mat.size) { IntArray(mat[0].size) }
        for (i in mat.indices) {
            dist[i].fill(Int.MAX_VALUE - 100000)
        }
        for (i in mat.indices) {
            for (j in mat[0].indices) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0
                } else {
                    if (i > 0) {
                        dist[i][j] = Math.min(dist[i][j], dist[i - 1][j] + 1)
                    }
                    if (j > 0) {
                        dist[i][j] = Math.min(dist[i][j], dist[i][j - 1] + 1)
                    }
                }
            }
        }
        for (i in mat.indices.reversed()) {
            for (j in mat[0].indices.reversed()) {
                if (i < mat.size - 1) {
                    dist[i][j] = Math.min(dist[i][j], dist[i + 1][j] + 1)
                }
                if (j < mat[0].size - 1) {
                    dist[i][j] = Math.min(dist[i][j], dist[i][j + 1] + 1)
                }
            }
        }
        return dist
    }
}
```