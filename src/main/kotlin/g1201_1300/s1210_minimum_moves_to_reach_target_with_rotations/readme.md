[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1210\. Minimum Moves to Reach Target with Rotations

Hard

In an `n*n` grid, there is a snake that spans 2 cells and starts moving from the top left corner at `(0, 0)` and `(0, 1)`. The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at `(n-1, n-2)` and `(n-1, n-1)`.

In one move the snake can:

*   Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
*   Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
*   Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from `(r, c)` and `(r, c+1)` to `(r, c)` and `(r+1, c)`.  
    ![](https://assets.leetcode.com/uploads/2019/09/24/image-2.png)
*   Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from `(r, c)` and `(r+1, c)` to `(r, c)` and `(r, c+1)`.  
    ![](https://assets.leetcode.com/uploads/2019/09/24/image-1.png)

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return `-1`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/24/image.png)**

**Input:** 

    grid = [ [0,0,0,0,0,1], 
            [1,1,0,0,1,0], 
            [0,0,0,0,1,1], 
            [0,0,1,0,1,0], 
            [0,1,1,0,0,0], 
            [0,1,1,0,0,0]]

**Output:** 11

**Explanation:** One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].

**Example 2:**

**Input:** 

    grid = [ [0,0,1,1,1,1], 
            [0,0,0,0,1,1], 
            [1,1,0,0,0,1], 
            [1,1,1,0,0,1], 
            [1,1,1,0,0,1], 
            [1,1,1,0,0,0]]

**Output:** 9

**Constraints:**

*   `2 <= n <= 100`
*   `0 <= grid[i][j] <= 1`
*   It is guaranteed that the snake starts at empty cells.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Objects
import java.util.Queue

class Solution {
    fun minimumMoves(grid: Array<IntArray>): Int {
        val n = grid.size
        val visited = Array(n) { IntArray(n) }
        val bq: Queue<IntArray> = LinkedList()
        bq.offer(intArrayOf(0, 0, 1))
        visited[0][0] = visited[0][0] or 1
        var level = 0
        while (bq.isNotEmpty()) {
            val levelSize = bq.size
            for (l in 0 until levelSize) {
                val cur = bq.poll()
                val xtail = Objects.requireNonNull(cur)[0]
                val ytail = cur[1]
                val dir = cur[2]
                if (xtail == n - 1 && ytail == n - 2 && dir == 1) {
                    return level
                }
                val xhead = xtail + if (dir == 1) 0 else 1
                val yhead = ytail + if (dir == 1) 1 else 0
                if (dir == 2) {
                    if (ytail + 1 < n && grid[xtail][ytail + 1] != 1 && grid[xtail + 1][ytail + 1] != 1) {
                        if (visited[xtail][ytail] and 1 == 0) {
                            bq.offer(intArrayOf(xtail, ytail, 1))
                            visited[xtail][ytail] = visited[xtail][ytail] or 1
                        }
                        if (visited[xtail][ytail + 1] and 2 == 0) {
                            bq.offer(intArrayOf(xtail, ytail + 1, 2))
                            visited[xtail][ytail + 1] = visited[xtail][ytail + 1] or 2
                        }
                    }
                    if (xhead + 1 < n && grid[xhead + 1][yhead] != 1 && visited[xhead][yhead] and 2 == 0) {
                        bq.offer(intArrayOf(xhead, yhead, 2))
                        visited[xhead][yhead] = visited[xhead][yhead] or 2
                    }
                } else {
                    if (xtail + 1 < n && grid[xtail + 1][ytail] != 1 && grid[xtail + 1][ytail + 1] != 1) {
                        if (visited[xtail][ytail] and 2 == 0) {
                            bq.offer(intArrayOf(xtail, ytail, 2))
                            visited[xtail][ytail] = visited[xtail][ytail] or 2
                        }
                        if (visited[xtail + 1][ytail] and 1 == 0) {
                            bq.offer(intArrayOf(xtail + 1, ytail, 1))
                            visited[xtail + 1][ytail] = visited[xtail + 1][ytail] or 1
                        }
                    }
                    if (yhead + 1 < n && grid[xhead][yhead + 1] != 1 && visited[xhead][yhead] and 1 == 0) {
                        bq.offer(intArrayOf(xhead, yhead, 1))
                        visited[xhead][yhead] = visited[xhead][yhead] or 1
                    }
                }
            }
            level += 1
        }
        return -1
    }
}
```