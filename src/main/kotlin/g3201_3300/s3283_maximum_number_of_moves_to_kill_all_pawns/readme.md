[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3283\. Maximum Number of Moves to Kill All Pawns

Hard

There is a `50 x 50` chessboard with **one** knight and some pawns on it. You are given two integers `kx` and `ky` where `(kx, ky)` denotes the position of the knight, and a 2D array `positions` where <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> denotes the position of the pawns on the chessboard.

Alice and Bob play a _turn-based_ game, where Alice goes first. In each player's turn:

*   The player _selects_ a pawn that still exists on the board and captures it with the knight in the **fewest** possible **moves**. **Note** that the player can select **any** pawn, it **might not** be one that can be captured in the **least** number of moves.
*   In the process of capturing the _selected_ pawn, the knight **may** pass other pawns **without** capturing them. **Only** the _selected_ pawn can be captured in _this_ turn.

Alice is trying to **maximize** the **sum** of the number of moves made by _both_ players until there are no more pawns on the board, whereas Bob tries to **minimize** them.

Return the **maximum** _total_ number of moves made during the game that Alice can achieve, assuming both players play **optimally**.

Note that in one **move,** a chess knight has eight possible positions it can move to, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2024/08/01/chess_knight.jpg)

**Example 1:**

**Input:** kx = 1, ky = 1, positions = \[\[0,0]]

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/16/gif3.gif)

The knight takes 4 moves to reach the pawn at `(0, 0)`.

**Example 2:**

**Input:** kx = 0, ky = 2, positions = \[\[1,1],[2,2],[3,3]]

**Output:** 8

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/08/16/gif4.gif)**

*   Alice picks the pawn at `(2, 2)` and captures it in two moves: `(0, 2) -> (1, 4) -> (2, 2)`.
*   Bob picks the pawn at `(3, 3)` and captures it in two moves: `(2, 2) -> (4, 1) -> (3, 3)`.
*   Alice picks the pawn at `(1, 1)` and captures it in four moves: `(3, 3) -> (4, 1) -> (2, 2) -> (0, 3) -> (1, 1)`.

**Example 3:**

**Input:** kx = 0, ky = 0, positions = \[\[1,2],[2,4]]

**Output:** 3

**Explanation:**

*   Alice picks the pawn at `(2, 4)` and captures it in two moves: `(0, 0) -> (1, 2) -> (2, 4)`. Note that the pawn at `(1, 2)` is not captured.
*   Bob picks the pawn at `(1, 2)` and captures it in one move: `(2, 4) -> (1, 2)`.

**Constraints:**

*   `0 <= kx, ky <= 49`
*   `1 <= positions.length <= 15`
*   `positions[i].length == 2`
*   `0 <= positions[i][0], positions[i][1] <= 49`
*   All `positions[i]` are unique.
*   The input is generated such that `positions[i] != [kx, ky]` for all `0 <= i < positions.length`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue
import kotlin.math.max
import kotlin.math.min

class Solution {
    private lateinit var distances: Array<IntArray>
    private lateinit var memo: Array<Array<Int?>?>

    fun maxMoves(kx: Int, ky: Int, positions: Array<IntArray>): Int {
        val n = positions.size
        distances = Array<IntArray>(n + 1) { IntArray(n + 1) { 0 } }
        memo = Array<Array<Int?>?>(n + 1) { arrayOfNulls<Int>(1 shl n) }
        // Calculate distances between all pairs of positions (including knight's initial position)
        for (i in 0 until n) {
            distances[n][i] = calculateMoves(kx, ky, positions[i][0], positions[i][1])
            for (j in i + 1 until n) {
                val dist =
                    calculateMoves(
                        positions[i][0],
                        positions[i][1],
                        positions[j][0],
                        positions[j][1],
                    )
                distances[j][i] = dist
                distances[i][j] = distances[j][i]
            }
        }
        return minimax(n, (1 shl n) - 1, true)
    }

    private fun minimax(lastPos: Int, remainingPawns: Int, isAlice: Boolean): Int {
        if (remainingPawns == 0) {
            return 0
        }
        if (memo[lastPos]!![remainingPawns] != null) {
            return memo[lastPos]!![remainingPawns]!!
        }
        var result = if (isAlice) 0 else Int.Companion.MAX_VALUE
        for (i in 0 until distances.size - 1) {
            if ((remainingPawns and (1 shl i)) != 0) {
                val newRemainingPawns = remainingPawns and (1 shl i).inv()
                val moveValue = distances[lastPos][i] + minimax(i, newRemainingPawns, !isAlice)
                result = if (isAlice) {
                    max(result, moveValue)
                } else {
                    min(result, moveValue)
                }
            }
        }
        memo[lastPos]!![remainingPawns] = result
        return result
    }

    private fun calculateMoves(x1: Int, y1: Int, x2: Int, y2: Int): Int {
        if (x1 == x2 && y1 == y2) {
            return 0
        }
        val visited = Array<BooleanArray?>(50) { BooleanArray(50) }
        val queue: Queue<IntArray> = LinkedList<IntArray>()
        queue.offer(intArrayOf(x1, y1, 0))
        visited[x1]!![y1] = true
        while (queue.isNotEmpty()) {
            val current = queue.poll()
            val x = current[0]
            val y = current[1]
            val moves = current[2]
            for (move in KNIGHT_MOVES) {
                val nx = x + move[0]
                val ny = y + move[1]
                if (nx == x2 && ny == y2) {
                    return moves + 1
                }
                if (nx >= 0 && nx < 50 && ny >= 0 && ny < 50 && !visited[nx]!![ny]) {
                    queue.offer(intArrayOf(nx, ny, moves + 1))
                    visited[nx]!![ny] = true
                }
            }
        }
        // Should never reach here if input is valid
        return -1
    }

    companion object {
        private val KNIGHT_MOVES = arrayOf<IntArray>(
            intArrayOf(-2, -1),
            intArrayOf(-2, 1),
            intArrayOf(-1, -2),
            intArrayOf(-1, 2),
            intArrayOf(1, -2),
            intArrayOf(1, 2),
            intArrayOf(2, -1),
            intArrayOf(2, 1),
        )
    }
}
```