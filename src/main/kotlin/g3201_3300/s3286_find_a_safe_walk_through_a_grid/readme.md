[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3286\. Find a Safe Walk Through a Grid

Medium

You are given an `m x n` binary matrix `grid` and an integer `health`.

You start on the upper-left corner `(0, 0)` and would like to get to the lower-right corner `(m - 1, n - 1)`.

You can move up, down, left, or right from one cell to another adjacent cell as long as your health _remains_ **positive**.

Cells `(i, j)` with `grid[i][j] = 1` are considered **unsafe** and reduce your health by 1.

Return `true` if you can reach the final cell with a health value of 1 or more, and `false` otherwise.

**Example 1:**

**Input:** grid = \[\[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]], health = 1

**Output:** true

**Explanation:**

The final cell can be reached safely by walking along the gray cells below.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_1drawio.png)

**Example 2:**

**Input:** grid = \[\[0,1,1,0,0,0],[1,0,1,0,0,0],[0,1,1,1,0,1],[0,0,1,0,1,0]], health = 3

**Output:** false

**Explanation:**

A minimum of 4 health points is needed to reach the final cell safely.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_2drawio.png)

**Example 3:**

**Input:** grid = \[\[1,1,1],[1,0,1],[1,1,1]], health = 5

**Output:** true

**Explanation:**

The final cell can be reached safely by walking along the gray cells below.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_3drawio.png)

Any path that does not go through the cell `(1, 1)` is unsafe since your health will drop to 0 when reaching the final cell.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 50`
*   `2 <= m * n`
*   `1 <= health <= m + n`
*   `grid[i][j]` is either 0 or 1.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Objects
import java.util.Queue

class Solution {
    fun findSafeWalk(grid: List<List<Int>>, health: Int): Boolean {
        val n = grid.size
        val m = grid[0].size
        val dr = intArrayOf(0, 0, 1, -1)
        val dc = intArrayOf(1, -1, 0, 0)
        val visited = Array<Array<BooleanArray>>(n) { Array<BooleanArray>(m) { BooleanArray(health + 1) } }
        val bfs: Queue<IntArray?> = LinkedList<IntArray>()
        bfs.add(intArrayOf(0, 0, health - grid[0][0]))
        visited[0][0][health - grid[0][0]] = true
        while (bfs.isNotEmpty()) {
            var size = bfs.size
            while (size-- > 0) {
                val currNode = bfs.poll()
                val r = Objects.requireNonNull<IntArray>(currNode)[0]
                val c = currNode!![1]
                val h = currNode[2]
                if (r == n - 1 && c == m - 1 && h > 0) {
                    return true
                }
                for (k in 0..3) {
                    val nr = r + dr[k]
                    val nc = c + dc[k]
                    if (isValidMove(nr, nc, n, m)) {
                        val nh: Int = h - grid[nr][nc]
                        if (nh >= 0 && !visited[nr][nc][nh]) {
                            visited[nr][nc][nh] = true
                            bfs.add(intArrayOf(nr, nc, nh))
                        }
                    }
                }
            }
        }
        return false
    }

    private fun isValidMove(r: Int, c: Int, n: Int, m: Int): Boolean {
        return r >= 0 && c >= 0 && r < n && c < m
    }
}
```