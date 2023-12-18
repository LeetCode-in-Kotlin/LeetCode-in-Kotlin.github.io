[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2850\. Minimum Moves to Spread Stones Over Grid

Medium

You are given a **0-indexed** 2D integer matrix `grid` of size `3 * 3`, representing the number of stones in each cell. The grid contains exactly `9` stones, and there can be **multiple** stones in a single cell.

In one move, you can move a single stone from its current cell to any other cell if the two cells share a side.

Return _the **minimum number of moves** required to place one stone in each cell_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg)

**Input:** grid = \[\[1,1,0],[1,1,1],[1,2,1]]

**Output:** 3

**Explanation:** One possible sequence of moves to place one stone in each cell is: 

1- Move one stone from cell (2,1) to cell (2,2). 

2- Move one stone from cell (2,2) to cell (1,2). 

3- Move one stone from cell (1,2) to cell (0,2). 

In total, it takes 3 moves to place one stone in each cell of the grid. 

It can be shown that 3 is the minimum number of moves required to place one stone in each cell.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg)

**Input:** grid = \[\[1,3,0],[1,0,0],[1,0,3]]

**Output:** 4

**Explanation:** One possible sequence of moves to place one stone in each cell is: 

1- Move one stone from cell (0,1) to cell (0,2). 

2- Move one stone from cell (0,1) to cell (1,1). 

3- Move one stone from cell (2,2) to cell (1,2). 

4- Move one stone from cell (2,2) to cell (2,1). 

In total, it takes 4 moves to place one stone in each cell of the grid. 

It can be shown that 4 is the minimum number of moves required to place one stone in each cell.

**Constraints:**

*   `grid.length == grid[i].length == 3`
*   `0 <= grid[i][j] <= 9`
*   Sum of `grid` is equal to `9`.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minimumMoves(grid: Array<IntArray>): Int {
        val a = grid[0][0] - 1
        val b = grid[0][1] - 1
        val c = grid[0][2] - 1
        val d = grid[1][0] - 1
        val f = grid[1][2] - 1
        val g = grid[2][0] - 1
        val h = grid[2][1] - 1
        val i = grid[2][2] - 1
        var minCost = Int.MAX_VALUE
        for (x in min(a, 0)..max(a, 0)) {
            for (y in min(c, 0)..max(c, 0)) {
                for (z in min(i, 0)..max(i, 0)) {
                    for (t in min(g, 0)..max(g, 0)) {
                        val cost: Int =
                            abs(x) + abs(y) + abs(z) + abs(t) + abs((x - a)) + abs(
                                (y - c)
                            ) + abs((z - i)) + abs((t - g)) + abs((x - y + b + c)) + abs(
                                (y - z + i + f)
                            ) + abs((z - t + g + h)) + abs((t - x + a + d))
                        if (cost < minCost) {
                            minCost = cost
                        }
                    }
                }
            }
        }
        return minCost
    }
}
```