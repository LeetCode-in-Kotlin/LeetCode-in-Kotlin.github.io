[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 827\. Making A Large Island

Hard

You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return _the size of the largest **island** in_ `grid` _after applying this operation_.

An **island** is a 4-directionally connected group of `1`s.

**Example 1:**

**Input:** grid = \[\[1,0],[0,1]]

**Output:** 3

**Explanation:** Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

**Example 2:**

**Input:** grid = \[\[1,1],[1,0]]

**Output:** 4

**Explanation:** Change the 0 to 1 and make the island bigger, only one island with area = 4.

**Example 3:**

**Input:** grid = \[\[1,1],[1,1]]

**Output:** 4

**Explanation:** Can't change any 0 to 1, only one island with area = 4.

**Constraints:**

*   `n == grid.length`
*   `n == grid[i].length`
*   `1 <= n <= 500`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    private lateinit var p: IntArray
    private lateinit var s: IntArray

    private fun makeSet(x: Int, y: Int, rl: Int) {
        val a = x * rl + y
        p[a] = a
        s[a] = 1
    }

    private fun comb(x1: Int, y1: Int, x2: Int, y2: Int, rl: Int) {
        var a = find(x1 * rl + y1)
        var b = find(x2 * rl + y2)
        if (a == b) {
            return
        }
        if (s[a] < s[b]) {
            val t = a
            a = b
            b = t
        }
        p[b] = a
        s[a] += s[b]
    }

    private fun find(a: Int): Int {
        if (p[a] == a) {
            return a
        }
        p[a] = find(p[a])
        return p[a]
    }

    fun largestIsland(grid: Array<IntArray>): Int {
        val rl = grid.size
        val cl = grid[0].size
        p = IntArray(rl * cl)
        s = IntArray(rl * cl)
        for (i in 0 until rl) {
            for (j in 0 until cl) {
                if (grid[i][j] == 0) {
                    continue
                }
                makeSet(i, j, rl)
                if (i > 0 && grid[i - 1][j] == 1) {
                    comb(i, j, i - 1, j, rl)
                }
                if (j > 0 && grid[i][j - 1] == 1) {
                    comb(i, j, i, j - 1, rl)
                }
            }
        }
        var m = 0
        var t: Int
        val sz: HashMap<Int, Int> = HashMap()
        for (i in 0 until rl) {
            for (j in 0 until cl) {
                if (grid[i][j] == 0) {
                    // find root, check if same and combine size
                    t = 1
                    if (i > 0 && grid[i - 1][j] == 1) {
                        sz[find((i - 1) * rl + j)] = s[find((i - 1) * rl + j)]
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        sz[find(i * rl + j - 1)] = s[find(i * rl + j - 1)]
                    }
                    if (i < rl - 1 && grid[i + 1][j] == 1) {
                        sz[find((i + 1) * rl + j)] = s[find((i + 1) * rl + j)]
                    }
                    if (j < cl - 1 && grid[i][j + 1] == 1) {
                        sz[find(i * rl + j + 1)] = s[find(i * rl + j + 1)]
                    }
                    for (`val` in sz.values) {
                        t += `val`
                    }
                    m = m.coerceAtLeast(t)
                    sz.clear()
                } else {
                    m = m.coerceAtLeast(s[i * rl + j])
                }
            }
        }
        return m
    }
}
```