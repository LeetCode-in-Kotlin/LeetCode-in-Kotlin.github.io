[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1284\. Minimum Number of Flips to Convert Binary Matrix to Zero Matrix

Hard

Given a `m x n` binary matrix `mat`. In one step, you can choose one cell and flip it and all the four neighbors of it if they exist (Flip is changing `1` to `0` and `0` to `1`). A pair of cells are called neighbors if they share one edge.

Return the _minimum number of steps_ required to convert `mat` to a zero matrix or `-1` if you cannot.

A **binary matrix** is a matrix with all cells equal to `0` or `1` only.

A **zero matrix** is a matrix with all cells equal to `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/11/28/matrix.png)

**Input:** mat = \[\[0,0],[0,1]]

**Output:** 3

**Explanation:** One possible solution is to flip (1, 0) then (0, 1) and finally (1, 1) as shown.

**Example 2:**

**Input:** mat = \[\[0]]

**Output:** 0

**Explanation:** Given matrix is a zero matrix. We do not need to change it.

**Example 3:**

**Input:** mat = \[\[1,0,0],[1,0,0]]

**Output:** -1

**Explanation:** Given matrix cannot be a zero matrix.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 3`
*   `mat[i][j]` is either `0` or `1`.

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Queue

class Solution {
    private lateinit var visited: MutableSet<Int>

    private fun isValid(x: Int, y: Int, r: Int, c: Int): Boolean {
        return x >= 0 && y >= 0 && x < r && y < c
    }

    private fun next(n: Int, r: Int, c: Int): List<Int> {
        val ans: MutableList<Int> = ArrayList()
        val dx = intArrayOf(0, 0, 0, 1, -1)
        val dy = intArrayOf(0, 1, -1, 0, 0)
        for (i in 0 until r) {
            for (j in 0 until c) {
                var newMask = n
                for (k in dx.indices) {
                    val nx = i + dx[k]
                    val ny = j + dy[k]
                    if (isValid(nx, ny, r, c)) {
                        newMask = newMask xor (1 shl nx * 3 + ny)
                    }
                }
                if (visited.add(newMask)) {
                    ans.add(newMask)
                }
            }
        }
        return ans
    }

    fun minFlips(mat: Array<IntArray>): Int {
        var mask = 0
        val r = mat.size
        val c = mat[0].size
        if (r == 1 && c == 1) {
            return if (mat[0][0] == 0) 0 else 1
        }
        for (i in 0 until r) {
            for (j in 0 until c) {
                mask = mask or (mat[i][j] shl i * 3 + j)
            }
        }
        if (mask == 0) {
            return 0
        }
        visited = HashSet()
        val q: Queue<Int> = ArrayDeque()
        var count = 1
        q.add(mask)
        visited.add(mask)
        while (q.isNotEmpty()) {
            val qSize = q.size
            for (i in 0 until qSize) {
                val currMask = q.poll()
                val nextStates = next(currMask, r, c)
                for (nextState in nextStates) {
                    if (nextState == 0) {
                        return count
                    }
                    q.add(nextState)
                }
            }
            count++
        }
        return -1
    }
}
```