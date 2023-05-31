[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1033\. Moving Stones Until Consecutive

Medium

There are three stones in different positions on the X-axis. You are given three integers `a`, `b`, and `c`, the positions of the stones.

In one move, you pick up a stone at an endpoint (i.e., either the lowest or highest position stone), and move it to an unoccupied position between those endpoints. Formally, let's say the stones are currently at positions `x`, `y`, and `z` with `x < y < z`. You pick up the stone at either position `x` or position `z`, and move that stone to an integer position `k`, with `x < k < z` and `k != y`.

The game ends when you cannot make any more moves (i.e., the stones are in three consecutive positions).

Return _an integer array_ `answer` _of length_ `2` _where_:

*   `answer[0]` _is the minimum number of moves you can play, and_
*   `answer[1]` _is the maximum number of moves you can play_.

**Example 1:**

**Input:** a = 1, b = 2, c = 5

**Output:** [1,2]

**Explanation:** Move the stone from 5 to 3, or move the stone from 5 to 4 to 3.

**Example 2:**

**Input:** a = 4, b = 3, c = 2

**Output:** [0,0]

**Explanation:** We cannot make any moves.

**Example 3:**

**Input:** a = 3, b = 5, c = 1

**Output:** [1,2]

**Explanation:** Move the stone from 1 to 4; or move the stone from 1 to 2 to 4.

**Constraints:**

*   `1 <= a, b, c <= 100`
*   `a`, `b`, and `c` have different values.

## Solution

```kotlin
class Solution {
    private fun minMoves(x: Int, y: Int, z: Int): Int {
        if (x + 1 == y && y + 1 == z) {
            return 0
        }
        return if (y - x <= 2 || z - y <= 2) {
            1
        } else 2
    }

    private fun maxMoves(x: Int, z: Int): Int {
        return z - x - 2
    }

    fun numMovesStones(a: Int, b: Int, c: Int): IntArray {
        val t = intArrayOf(a, b, c)
        t.sort()
        val min = minMoves(t[0], t[1], t[2])
        val max = maxMoves(t[0], t[2])
        return intArrayOf(min, max)
    }
}
```