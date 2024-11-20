[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2056\. Number of Valid Move Combinations On Chessboard

Hard

There is an `8 x 8` chessboard containing `n` pieces (rooks, queens, or bishops). You are given a string array `pieces` of length `n`, where `pieces[i]` describes the type (rook, queen, or bishop) of the <code>i<sup>th</sup></code> piece. In addition, you are given a 2D integer array `positions` also of length `n`, where <code>positions[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> piece is currently at the **1-based** coordinate <code>(r<sub>i</sub>, c<sub>i</sub>)</code> on the chessboard.

When making a **move** for a piece, you choose a **destination** square that the piece will travel toward and stop on.

*   A rook can only travel **horizontally or vertically** from `(r, c)` to the direction of `(r+1, c)`, `(r-1, c)`, `(r, c+1)`, or `(r, c-1)`.
*   A queen can only travel **horizontally, vertically, or diagonally** from `(r, c)` to the direction of `(r+1, c)`, `(r-1, c)`, `(r, c+1)`, `(r, c-1)`, `(r+1, c+1)`, `(r+1, c-1)`, `(r-1, c+1)`, `(r-1, c-1)`.
*   A bishop can only travel **diagonally** from `(r, c)` to the direction of `(r+1, c+1)`, `(r+1, c-1)`, `(r-1, c+1)`, `(r-1, c-1)`.

You must make a **move** for every piece on the board simultaneously. A **move combination** consists of all the **moves** performed on all the given pieces. Every second, each piece will instantaneously travel **one square** towards their destination if they are not already at it. All pieces start traveling at the <code>0<sup>th</sup></code> second. A move combination is **invalid** if, at a given time, **two or more** pieces occupy the same square.

Return _the number of **valid** move combinations_.

**Notes:**

*   **No two pieces** will start in the **same** square.
*   You may choose the square a piece is already on as its **destination**.
*   If two pieces are **directly adjacent** to each other, it is valid for them to **move past each other** and swap positions in one second.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/23/a1.png)

**Input:** pieces = ["rook"], positions = \[\[1,1]]

**Output:** 15

**Explanation:** The image above shows the possible squares the piece can move to. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/23/a2.png)

**Input:** pieces = ["queen"], positions = \[\[1,1]]

**Output:** 22

**Explanation:** The image above shows the possible squares the piece can move to. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/23/a3.png)

**Input:** pieces = ["bishop"], positions = \[\[4,3]]

**Output:** 12

**Explanation:** The image above shows the possible squares the piece can move to. 

**Constraints:**

*   `n == pieces.length`
*   `n == positions.length`
*   `1 <= n <= 4`
*   `pieces` only contains the strings `"rook"`, `"queen"`, and `"bishop"`.
*   There will be at most one queen on the chessboard.
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> <= 8</code>
*   Each `positions[i]` is distinct.

## Solution

```kotlin
class Solution {
    // 0: rook, queen, bishop
    private val dirs = arrayOf(
        arrayOf(intArrayOf(-1, 0), intArrayOf(1, 0), intArrayOf(0, -1), intArrayOf(0, 1)),
        arrayOf(
            intArrayOf(-1, 0),
            intArrayOf(1, 0),
            intArrayOf(0, -1),
            intArrayOf(0, 1),
            intArrayOf(1, 1),
            intArrayOf(-1, -1),
            intArrayOf(-1, 1),
            intArrayOf(1, -1),
        ),
        arrayOf(intArrayOf(1, 1), intArrayOf(-1, -1), intArrayOf(-1, 1), intArrayOf(1, -1)),
    )

    fun countCombinations(pieces: Array<String?>, positions: Array<IntArray>): Int {
        val endPosition: Array<ArrayList<IntArray>?> = arrayOfNulls(pieces.size)
        for (i in pieces.indices) {
            endPosition[i] = ArrayList()
        }
        for (i in pieces.indices) {
            positions[i][0]--
            positions[i][1]--
            endPosition[i]!!.add(positions[i])
            var dirIndex = 0
            when (pieces[i]) {
                "rook" -> dirIndex = 0
                "queen" -> dirIndex = 1
                "bishop" -> dirIndex = 2
            }
            for (d in dirs[dirIndex]) {
                var r = positions[i][0]
                var c = positions[i][1]
                while (true) {
                    r += d[0]
                    c += d[1]
                    if (r < 0 || r >= 8 || c < 0 || c >= 8) {
                        break
                    }
                    endPosition[i]!!.add(intArrayOf(r, c))
                }
            }
        }
        return dfs(positions, endPosition, IntArray(pieces.size), 0)
    }

    private fun dfs(positions: Array<IntArray>, stop: Array<ArrayList<IntArray>?>, stopIndex: IntArray, cur: Int): Int {
        if (cur == stopIndex.size) {
            val p = Array(positions.size) { IntArray(2) }
            for (i in p.indices) {
                p[i] = intArrayOf(positions[i][0], positions[i][1])
            }
            return check(p, stop, stopIndex)
        }
        var res = 0
        for (i in stop[cur]!!.indices) {
            stopIndex[cur] = i
            res += dfs(positions, stop, stopIndex, cur + 1)
        }
        return res
    }

    private fun check(positions: Array<IntArray>, stop: Array<ArrayList<IntArray>?>, stopIndex: IntArray): Int {
        var keepGoing = true
        while (keepGoing) {
            keepGoing = false
            for (i in positions.indices) {
                var diff = stop[i]!![stopIndex[i]][0] - positions[i][0]
                if (diff > 0) {
                    keepGoing = true
                    positions[i][0]++
                } else if (diff < 0) {
                    keepGoing = true
                    positions[i][0]--
                }
                diff = stop[i]!![stopIndex[i]][1] - positions[i][1]
                if (diff > 0) {
                    keepGoing = true
                    positions[i][1]++
                } else if (diff < 0) {
                    keepGoing = true
                    positions[i][1]--
                }
            }
            val seen: MutableSet<Int> = HashSet()
            for (position in positions) {
                val key = position[0] * 100 + position[1]
                if (seen.contains(key)) {
                    return 0
                }
                seen.add(key)
            }
        }
        return 1
    }
}
```