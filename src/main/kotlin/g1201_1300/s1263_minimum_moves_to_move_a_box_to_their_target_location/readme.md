[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1263\. Minimum Moves to Move a Box to Their Target Location

Hard

A storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by an `m x n` grid of characters `grid` where each element is a wall, floor, or box.

Your task is to move the box `'B'` to the target position `'T'` under the following rules:

*   The character `'S'` represents the player. The player can move up, down, left, right in `grid` if it is a floor (empty cell).
*   The character `'.'` represents the floor which means a free cell to walk.
*   The character `'#'` represents the wall which means an obstacle (impossible to walk there).
*   There is only one box `'B'` and one target cell `'T'` in the `grid`.
*   The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a **push**.
*   The player cannot walk through the box.

Return _the minimum number of **pushes** to move the box to the target_. If there is no way to reach the target, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/11/06/sample_1_1620.png)

**Input:**

    grid = [ ["#","#","#","#","#","#"],
            ["#","T","#","#","#","#"],
            ["#",".",".","B",".","#"],
            ["#",".","#","#",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** 3

**Explanation:** We return only the number of times the box is pushed.

**Example 2:**

**Input:**

    grid = [ ["#","#","#","#","#","#"],
            ["#","T","#","#","#","#"],
            ["#",".",".","B",".","#"],
            ["#","#","#","#",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** -1

**Example 3:**

**Input:**

    grid = [ ["#","#","#","#","#","#"],
            ["#","T",".",".","#","#"],
            ["#",".","#","B",".","#"],
            ["#",".",".",".",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** 5

**Explanation:** push the box down, left, left, up and up.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 20`
*   `grid` contains only characters `'.'`, `'#'`, `'S'`, `'T'`, or `'B'`.
*   There is only one character `'S'`, `'B'`, and `'T'` in the `grid`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    private var n = 0
    private var m = 0
    private lateinit var grid: Array<CharArray>
    private val dirs = arrayOf(intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1))

    fun minPushBox(grid: Array<CharArray>): Int {
        n = grid.size
        m = grid[0].size
        this.grid = grid
        val box = IntArray(2)
        val target = IntArray(2)
        val player = IntArray(2)
        findLocations(box, target, player)
        val q: Queue<IntArray> = LinkedList()
        q.offer(intArrayOf(box[0], box[1], player[0], player[1]))
        // for 4 directions
        val visited = Array(n) { Array(m) { BooleanArray(4) } }
        var steps = 0
        while (q.isNotEmpty()) {
            var size = q.size
            while (size-- > 0) {
                val cur = q.poll()
                if (cur != null && cur[0] == target[0] && cur[1] == target[1]) {
                    return steps
                }
                for (i in 0..3) {
                    if (cur != null) {
                        val newPlayerLoc = intArrayOf(cur[0] + dirs[i][0], cur[1] + dirs[i][1])
                        val newBoxLoc = intArrayOf(cur[0] - dirs[i][0], cur[1] - dirs[i][1])
                        if (visited[cur[0]][cur[1]][i] ||
                            isOutOfBounds(newPlayerLoc, newBoxLoc) ||
                            !isReachable(newPlayerLoc, cur)
                        ) {
                            continue
                        }
                        visited[cur[0]][cur[1]][i] = true
                        q.offer(intArrayOf(newBoxLoc[0], newBoxLoc[1], cur[0], cur[1]))
                    }
                }
            }
            steps++
        }
        return -1
    }

    private fun isReachable(targetPlayerLoc: IntArray, cur: IntArray): Boolean {
        val visited = Array(n) { BooleanArray(m) }
        visited[cur[0]][cur[1]] = true
        visited[cur[2]][cur[3]] = true
        val q: Queue<IntArray> = LinkedList()
        q.offer(intArrayOf(cur[2], cur[3]))
        while (q.isNotEmpty()) {
            val playerLoc = q.poll()
            if (playerLoc[0] == targetPlayerLoc[0] && playerLoc[1] == targetPlayerLoc[1]) {
                return true
            }
            for (d in dirs) {
                val x = playerLoc[0] + d[0]
                val y = playerLoc[1] + d[1]
                if (isOutOfBounds(x, y) || visited[x][y]) {
                    continue
                }
                visited[x][y] = true
                q.offer(intArrayOf(x, y))
            }
        }
        return false
    }

    private fun isOutOfBounds(player: IntArray, box: IntArray): Boolean {
        return isOutOfBounds(player[0], player[1]) || isOutOfBounds(box[0], box[1])
    }

    private fun isOutOfBounds(x: Int, y: Int): Boolean {
        return x < 0 || y < 0 || x == n || y == m || grid[x][y] == '#'
    }

    private fun findLocations(box: IntArray, target: IntArray, player: IntArray) {
        var p = false
        var t = false
        var b = false
        for (i in 0 until n) {
            for (j in 0 until m) {
                if (grid[i][j] == 'S') {
                    player[0] = i
                    player[1] = j
                    p = true
                } else if (grid[i][j] == 'T') {
                    target[0] = i
                    target[1] = j
                    t = true
                } else if (grid[i][j] == 'B') {
                    box[0] = i
                    box[1] = j
                    b = true
                }
                if (p && b && t) {
                    // found all
                    return
                }
            }
        }
    }
}
```