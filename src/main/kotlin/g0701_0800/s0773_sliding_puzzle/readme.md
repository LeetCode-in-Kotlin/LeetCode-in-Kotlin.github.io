[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 773\. Sliding Puzzle

Hard

On an `2 x 3` board, there are five tiles labeled from `1` to `5`, and an empty square represented by `0`. A **move** consists of choosing `0` and a 4-directionally adjacent number and swapping it.

The state of the board is solved if and only if the board is `[[1,2,3],[4,5,0]]`.

Given the puzzle board `board`, return _the least number of moves required so that the state of the board is solved_. If it is impossible for the state of the board to be solved, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide1-grid.jpg)

**Input:** board = \[\[1,2,3],[4,0,5]]

**Output:** 1

**Explanation:** Swap the 0 and the 5 in one move.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide2-grid.jpg)

**Input:** board = \[\[1,2,3],[5,4,0]]

**Output:** -1

**Explanation:** No number of moves will make the board solved.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/06/29/slide3-grid.jpg)

**Input:** board = \[\[4,1,2],[5,0,3]]

**Output:** 5

**Explanation:** 5 is the smallest number of moves that solves the board. An example path: After move 0: [[4,1,2],[5,0,3]] After move 1: [[4,1,2],[0,5,3]] After move 2: [[0,1,2],[4,5,3]] After move 3: [[1,0,2],[4,5,3]] After move 4: [[1,2,0],[4,5,3]] After move 5: [[1,2,3],[4,5,0]]

**Constraints:**

*   `board.length == 2`
*   `board[i].length == 3`
*   `0 <= board[i][j] <= 5`
*   Each value `board[i][j]` is **unique**.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    private class Node(var board: String, var depth: Int, var y: Int, var x: Int)
    fun slidingPuzzle(board: Array<IntArray>): Int {
        val targetStr = "123450"
        val sb = StringBuilder()
        var y = 0
        var x = 0
        for (i in board.indices) {
            for (j in board[0].indices) {
                if (board[i][j] == 0) {
                    y = i
                    x = j
                }
                sb.append(board[i][j])
            }
        }
        val seen: MutableSet<String> = HashSet()
        val q: Queue<Node> = LinkedList()
        q.add(Node(sb.toString(), 0, y, x))
        val dir = arrayOf(intArrayOf(1, 0), intArrayOf(-1, 0), intArrayOf(0, 1), intArrayOf(0, -1))
        while (q.isNotEmpty()) {
            val next = q.poll()
            val s = next.board
            if (!seen.contains(s)) {
                if (s == targetStr) {
                    return next.depth
                }
                val nextDepth = next.depth + 1
                y = next.y
                x = next.x
                for (vector in dir) {
                    val nextY = y + vector[0]
                    val nextX = x + vector[1]
                    if (0 <= nextY && nextY < board.size && 0 <= nextX && nextX < board[0].size) {
                        val newBoard = swap(s, y, x, nextY, nextX)
                        q.add(Node(newBoard, nextDepth, nextY, nextX))
                    }
                }
                seen.add(s)
            }
        }
        return -1
    }

    private fun swap(board: String, y1: Int, x1: Int, y2: Int, x2: Int): String {
        val arr = board.toCharArray()
        val t = board[y1 * 3 + x1]
        arr[y1 * 3 + x1] = board[y2 * 3 + x2]
        arr[y2 * 3 + x2] = t
        return String(arr)
    }
}
```