[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1001\. Grid Illumination

Hard

There is a 2D `grid` of size `n x n` where each cell of this grid has a lamp that is initially **turned off**.

You are given a 2D array of lamp positions `lamps`, where <code>lamps[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> indicates that the lamp at <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code> is **turned on**. Even if the same lamp is listed more than once, it is turned on.

When a lamp is turned on, it **illuminates its cell** and **all other cells** in the same **row, column, or diagonal**.

You are also given another 2D array `queries`, where <code>queries[j] = [row<sub>j</sub>, col<sub>j</sub>]</code>. For the <code>j<sup>th</sup></code> query, determine whether <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> is illuminated or not. After answering the <code>j<sup>th</sup></code> query, **turn off** the lamp at <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> and its **8 adjacent lamps** if they exist. A lamp is adjacent if its cell shares either a side or corner with <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code>.

Return _an array of integers_ `ans`_,_ _where_ `ans[j]` _should be_ `1` _if the cell in the_ <code>j<sup>th</sup></code> _query was illuminated, or_ `0` _if the lamp was not._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/19/illu_1.jpg)

**Input:** n = 5, lamps = \[\[0,0],[4,4]], queries = \[\[1,1],[1,0]]

**Output:** [1,0]

**Explanation:** We have the initial grid with all lamps turned off. In the above picture we see the grid after turning on the lamp at grid[0][0] then turning on the lamp at grid[4][4]. The 0<sup>th</sup> query asks if the lamp at grid[1][1] is illuminated or not (the blue square). It is illuminated, so set ans[0] = 1. Then, we turn off all lamps in the red square. ![](https://assets.leetcode.com/uploads/2020/08/19/illu_step1.jpg) The 1<sup>st</sup> query asks if the lamp at grid[1][0] is illuminated or not (the blue square). It is not illuminated, so set ans[1] = 0. Then, we turn off all lamps in the red rectangle. ![](https://assets.leetcode.com/uploads/2020/08/19/illu_step2.jpg)

**Example 2:**

**Input:** n = 5, lamps = \[\[0,0],[4,4]], queries = \[\[1,1],[1,1]]

**Output:** [1,1]

**Example 3:**

**Input:** n = 5, lamps = \[\[0,0],[0,4]], queries = \[\[0,4],[0,1],[1,4]]

**Output:** [1,1,0]

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   `0 <= lamps.length <= 20000`
*   `0 <= queries.length <= 20000`
*   `lamps[i].length == 2`
*   <code>0 <= row<sub>i</sub>, col<sub>i</sub> < n</code>
*   `queries[j].length == 2`
*   <code>0 <= row<sub>j</sub>, col<sub>j</sub> < n</code>

## Solution

```kotlin
class Solution {
    fun gridIllumination(n: Int, lamps: Array<IntArray>, queries: Array<IntArray>): IntArray {
        val rowIlluminations: MutableMap<Int, Int> = HashMap()
        val colIlluminations: MutableMap<Int, Int> = HashMap()
        val posDiagIlluminations: MutableMap<Int, Int> = HashMap()
        val negDiagIlluminations: MutableMap<Int, Int> = HashMap()
        val lampPlacements: MutableSet<Long> = HashSet()
        for (lamp in lamps) {
            val row = lamp[0]
            val col = lamp[1]
            var key = row.toLong()
            key = key * n + col
            if (lampPlacements.contains(key)) {
                continue
            }
            incr(rowIlluminations, row)
            incr(colIlluminations, col)
            incr(posDiagIlluminations, row + col)
            incr(negDiagIlluminations, row + (n - 1 - col))
            lampPlacements.add(key)
        }
        val ans = IntArray(queries.size)
        for (i in ans.indices) {
            val row = queries[i][0]
            val col = queries[i][1]
            if (rowIlluminations.containsKey(row) ||
                colIlluminations.containsKey(col) ||
                posDiagIlluminations.containsKey(row + col) ||
                negDiagIlluminations.containsKey(row + (n - 1 - col))
            ) {
                ans[i] = 1
            }
            val topRow = 0.coerceAtLeast(row - 1)
            val bottomRow = (n - 1).coerceAtMost(row + 1)
            val leftCol = 0.coerceAtLeast(col - 1)
            val rightCol = (n - 1).coerceAtMost(col + 1)
            for (r in topRow..bottomRow) {
                for (c in leftCol..rightCol) {
                    var key = r.toLong()
                    key = key * n + c
                    if (lampPlacements.contains(key)) {
                        decr(rowIlluminations, r)
                        decr(colIlluminations, c)
                        decr(posDiagIlluminations, r + c)
                        decr(negDiagIlluminations, r + (n - 1 - c))
                        lampPlacements.remove(key)
                    }
                }
            }
        }
        return ans
    }

    private fun incr(map: MutableMap<Int, Int>, key: Int) {
        map[key] = map.getOrDefault(key, 0) + 1
    }

    private fun decr(map: MutableMap<Int, Int>, key: Int) {
        val v = map.getValue(key)
        if (map[key] == 1) {
            map.remove(key)
        } else {
            map[key] = v - 1
        }
    }
}
```