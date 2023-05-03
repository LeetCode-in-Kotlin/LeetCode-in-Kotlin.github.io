[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 959\. Regions Cut By Slashes

Medium

An `n x n` grid is composed of `1 x 1` squares where each `1 x 1` square consists of a `'/'`, `'\'`, or blank space `' '`. These characters divide the square into contiguous regions.

Given the grid `grid` represented as a string array, return _the number of regions_.

Note that backslash characters are escaped, so a `'\'` is represented as `'\\'`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/1.png)

**Input:** grid = [" /","/ "]

**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/2.png)

**Input:** grid = [" /"," "]

**Output:** 1

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/15/4.png)

**Input:** grid = ["/\\\\","\\\\/"]

**Output:** 5

**Explanation:** Recall that because \\ characters are escaped, "\\\\/" refers to \\/, and "/\\\\" refers to /\\.

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 30`
*   `grid[i][j]` is either `'/'`, `'\'`, or `' '`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var regions = 0
    private lateinit var parent: IntArray
    fun regionsBySlashes(grid: Array<String>): Int {
        val n = grid.size
        regions = n * n * 4
        unionFind(regions)
        for (row in grid.indices) {
            val str = grid[row]
            val ch = str.toCharArray()
            for ((col, c) in ch.withIndex()) {
                val index = row * n * 4 + col * 4
                when (c) {
                    '/' -> {
                        union(index, index + 3)
                        union(index + 1, index + 2)
                    }
                    ' ' -> {
                        union(index, index + 1)
                        union(index + 1, index + 2)
                        union(index + 2, index + 3)
                    } else -> {
                        union(index, index + 1)
                        union(index + 2, index + 3)
                    }
                }
                if (row != n - 1) {
                    union(index + 2, index + 4 * n)
                }
                if (col != n - 1) {
                    union(index + 1, index + 7)
                }
            }
        }
        return regions
    }

    private fun unionFind(n: Int) {
        parent = IntArray(n)
        for (i in 0 until n) {
            parent[i] = i
        }
    }

    private fun union(p: Int, q: Int) {
        if (connected(p, q)) {
            return
        }
        --regions
        val i = root(p)
        val j = root(q)
        if (i > j) {
            parent[i] = j
        } else {
            parent[j] = i
        }
    }

    private fun connected(p: Int, q: Int): Boolean {
        return root(p) == root(q)
    }

    private fun root(index: Int): Int {
        var index = index
        while (index != parent[index]) {
            parent[index] = parent[parent[index]]
            index = parent[index]
        }
        return index
    }
}
```