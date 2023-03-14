[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 749\. Contain Virus

Hard

A virus is spreading rapidly, and your task is to quarantine the infected area by installing walls.

The world is modeled as an `m x n` binary grid `isInfected`, where `isInfected[i][j] == 0` represents uninfected cells, and `isInfected[i][j] == 1` represents cells contaminated with the virus. A wall (and only one wall) can be installed between any two **4-directionally** adjacent cells, on the shared boundary.

Every night, the virus spreads to all neighboring cells in all four directions unless blocked by a wall. Resources are limited. Each day, you can install walls around only one region (i.e., the affected area (continuous block of infected cells) that threatens the most uninfected cells the following night). There **will never be a tie**.

Return _the number of walls used to quarantine all the infected regions_. If the world will become fully infected, return the number of walls used.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/virus11-grid.jpg)

**Input:** isInfected = \[\[0,1,0,0,0,0,0,1],[0,1,0,0,0,0,0,1],[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0]]

**Output:** 10

**Explanation:** There are 2 contaminated regions. On the first day, add 5 walls to quarantine the viral region on the left. The board after the virus spreads is: ![](https://assets.leetcode.com/uploads/2021/06/01/virus12edited-grid.jpg) On the second day, add 5 walls to quarantine the viral region on the right. The virus is fully contained. ![](https://assets.leetcode.com/uploads/2021/06/01/virus13edited-grid.jpg)

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/01/virus2-grid.jpg)

**Input:** isInfected = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** 4

**Explanation:** Even though there is only one cell saved, there are 4 walls built. Notice that walls are only built on the shared boundary of two different cells.

**Example 3:**

**Input:** isInfected = \[\[1,1,1,0,0,0,0,0,0],[1,0,1,0,1,1,1,1,1],[1,1,1,0,0,0,0,0,0]]

**Output:** 13

**Explanation:** The region on the left only builds two new walls.

**Constraints:**

*   `m == isInfected.length`
*   `n == isInfected[i].length`
*   `1 <= m, n <= 50`
*   `isInfected[i][j]` is either `0` or `1`.
*   There is always a contiguous viral region throughout the described process that will **infect strictly more uncontaminated squares** in the next round.

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    private var m = 0
    private var n = 0
    private val dirs = arrayOf(intArrayOf(-1, 0), intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(0, -1))

    fun containVirus(grid: Array<IntArray>): Int {
        m = grid.size
        n = grid[0].size
        var res = 0
        while (true) {
            var id = 0
            val visited: MutableSet<Int> = HashSet()
            val islands: MutableMap<Int, MutableSet<Int>> = HashMap()
            val scores: MutableMap<Int, MutableSet<Int>> = HashMap()
            val walls: MutableMap<Int, Int> = HashMap()
            for (i in 0 until m) {
                for (j in 0 until n) {
                    if (grid[i][j] == 1 && !visited.contains(i * n + j)) {
                        dfs(i, j, visited, grid, islands, scores, walls, id++)
                    }
                }
            }
            if (islands.isEmpty()) {
                break
            }
            var maxVirus = 0
            for (i in 0 until id) {
                if (scores.getOrDefault(maxVirus, HashSet()).size
                    < scores.getOrDefault(i, HashSet()).size
                ) {
                    maxVirus = i
                }
            }
            res += walls.getOrDefault(maxVirus, 0)
            for (i in 0 until islands.size) {
                for (island in islands[i]!!) {
                    val x = island / n
                    val y = island % n
                    if (i == maxVirus) {
                        grid[x][y] = -1
                    } else {
                        for (dir in dirs) {
                            val nx = x + dir[0]
                            val ny = y + dir[1]
                            if (nx in 0 until m && ny >= 0 && ny < n && grid[nx][ny] == 0) {
                                grid[nx][ny] = 1
                            }
                        }
                    }
                }
            }
        }
        return res
    }

    private fun dfs(
        i: Int,
        j: Int,
        visited: MutableSet<Int>,
        grid: Array<IntArray>,
        islands: MutableMap<Int, MutableSet<Int>>,
        scores: MutableMap<Int, MutableSet<Int>>,
        walls: MutableMap<Int, Int>,
        id: Int
    ) {
        if (!visited.add(i * n + j)) {
            return
        }
        islands.computeIfAbsent(id) { HashSet() }.add(i * n + j)
        for (dir in dirs) {
            val x = i + dir[0]
            val y = j + dir[1]
            if (x < 0 || x >= m || y < 0 || y >= n) {
                continue
            }
            if (grid[x][y] == 1) {
                dfs(x, y, visited, grid, islands, scores, walls, id)
            }
            if (grid[x][y] == 0) {
                scores.computeIfAbsent(
                    id
                ) { HashSet() }.add(x * n + y)
                walls[id] = walls.getOrDefault(id, 0) + 1
            }
        }
    }
}
```