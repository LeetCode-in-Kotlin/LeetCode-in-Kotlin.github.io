[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 675\. Cut Off Trees for Golf Event

Hard

You are asked to cut off all the trees in a forest for a golf event. The forest is represented as an `m x n` matrix. In this matrix:

*   `0` means the cell cannot be walked through.
*   `1` represents an empty cell that can be walked through.
*   A number greater than `1` represents a tree in a cell that can be walked through, and this number is the tree's height.

In one step, you can walk in any of the four directions: north, east, south, and west. If you are standing in a cell with a tree, you can choose whether to cut it off.

You must cut off the trees in order from shortest to tallest. When you cut off a tree, the value at its cell becomes `1` (an empty cell).

Starting from the point `(0, 0)`, return _the minimum steps you need to walk to cut off all the trees_. If you cannot cut off all the trees, return `-1`.

**Note:** The input is generated such that no two trees have the same height, and there is at least one tree needs to be cut off.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/trees1.jpg)

**Input:** forest = \[\[1,2,3],[0,0,4],[7,6,5]]

**Output:** 6

**Explanation:** Following the path above allows you to cut off the trees from shortest to tallest in 6 steps.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/trees2.jpg)

**Input:** forest = \[\[1,2,3],[0,0,0],[7,6,5]]

**Output:** -1

**Explanation:** The trees in the bottom row cannot be accessed as the middle row is blocked.

**Example 3:**

**Input:** forest = \[\[2,3,4],[0,0,5],[8,7,6]]

**Output:** 6

**Explanation:** You can follow the same path as Example 1 to cut off all the trees. Note that you can cut off the first tree at (0, 0) before making any steps.

**Constraints:**

*   `m == forest.length`
*   `n == forest[i].length`
*   `1 <= m, n <= 50`
*   <code>0 <= forest[i][j] <= 10<sup>9</sup></code>
*   Heights of all trees are **distinct**.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Objects
import java.util.PriorityQueue
import java.util.Queue

class Solution {
    private var r = 0
    private var c = 0
    fun cutOffTree(forest: List<List<Int>>): Int {
        val pq = PriorityQueue<Int>()
        for (integers in forest) {
            for (v in integers) {
                if (v > 1) {
                    pq.add(v)
                }
            }
        }
        var steps = 0
        while (!pq.isEmpty()) {
            val count = minSteps(forest, pq.poll())
            if (count == -1) {
                return -1
            }
            steps += count
        }
        return steps
    }

    private fun minSteps(forest: List<List<Int>>, target: Int): Int {
        var steps = 0
        val dirs = arrayOf(intArrayOf(0, 1), intArrayOf(0, -1), intArrayOf(1, 0), intArrayOf(-1, 0))
        val visited = Array(forest.size) {
            BooleanArray(
                forest[0].size
            )
        }
        val q: Queue<IntArray> = LinkedList()
        q.add(intArrayOf(r, c))
        visited[r][c] = true
        while (!q.isEmpty()) {
            val qSize = q.size
            for (i in 0 until qSize) {
                val curr = q.poll()
                if (forest[Objects.requireNonNull(curr)[0]][curr[1]] == target) {
                    r = curr[0]
                    c = curr[1]
                    return steps
                }
                for (k in 0..3) {
                    val dir = dirs[k]
                    val nr = dir[0] + curr[0]
                    val nc = dir[1] + curr[1]
                    if (nr < 0 || nr == visited.size || nc < 0 || nc == visited[0].size ||
                        visited[nr][nc] || forest[nr][nc] == 0
                    ) {
                        continue
                    }
                    q.add(intArrayOf(nr, nc))
                    visited[nr][nc] = true
                }
            }
            steps++
        }
        return -1
    }
}
```