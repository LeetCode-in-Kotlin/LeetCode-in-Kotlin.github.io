[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3311\. Construct 2D Grid Matching Graph Layout

Hard

You are given a 2D integer array `edges` representing an **undirected** graph having `n` nodes, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> denotes an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

Construct a 2D grid that satisfies these conditions:

*   The grid contains **all nodes** from `0` to `n - 1` in its cells, with each node appearing exactly **once**.
*   Two nodes should be in adjacent grid cells (**horizontally** or **vertically**) **if and only if** there is an edge between them in `edges`.

It is guaranteed that `edges` can form a 2D grid that satisfies the conditions.

Return a 2D integer array satisfying the conditions above. If there are multiple solutions, return _any_ of them.

**Example 1:**

**Input:** n = 4, edges = \[\[0,1],[0,2],[1,3],[2,3]]

**Output:** [[3,1],[2,0]]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/11/screenshot-from-2024-08-11-14-07-59.png)

**Example 2:**

**Input:** n = 5, edges = \[\[0,1],[1,3],[2,3],[2,4]]

**Output:** [[4,2,3,1,0]]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/11/screenshot-from-2024-08-11-14-06-02.png)

**Example 3:**

**Input:** n = 9, edges = \[\[0,1],[0,4],[0,5],[1,7],[2,3],[2,4],[2,5],[3,6],[4,6],[4,7],[6,8],[7,8]]

**Output:** [[8,6,3],[7,4,2],[1,0,5]]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/11/screenshot-from-2024-08-11-14-06-38.png)

**Constraints:**

*   <code>2 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= edges.length <= 10<sup>5</sup></code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub> < v<sub>i</sub> < n</code>
*   All the edges are distinct.
*   The input is generated such that `edges` can form a 2D grid that satisfies the conditions.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun constructGridLayout(n: Int, edges: Array<IntArray>): Array<IntArray> {
        val cs = IntArray(n)
        val als: Array<ArrayList<Int>> = Array<ArrayList<Int>>(n) { ArrayList<Int>() }
        for (e in edges) {
            cs[e[0]]++
            cs[e[1]]++
            als[e[0]].add(e[1])
            als[e[1]].add(e[0])
        }
        var min = 4
        for (a in cs) {
            min = min(min, a)
        }
        val seen = BooleanArray(n)
        var res: Array<IntArray>
        var st = 0
        for (i in 0 until n) {
            if (cs[i] == min) {
                st = i
                break
            }
        }
        if (min == 1) {
            res = Array<IntArray>(1) { IntArray(n) }
            for (i in 0 until n) {
                res[0][i] = st
                seen[st] = true
                if (i + 1 < n) {
                    for (a in als[st]) {
                        if (!seen[a]) {
                            st = a
                            break
                        }
                    }
                }
            }
            return res
        }
        var row2 = -1
        for (a in als[st]) {
            if (cs[a] == min) {
                row2 = a
                break
            }
        }
        if (row2 >= 0) {
            return getInts2(n, st, row2, seen, als)
        }
        return getInts1(n, seen, st, als, cs)
    }

    private fun getInts1(
        n: Int,
        seen: BooleanArray,
        st: Int,
        als: Array<ArrayList<Int>>,
        cs: IntArray,
    ): Array<IntArray> {
        var st = st
        var res: Array<IntArray>
        val al = ArrayList<Int?>()
        var f = true
        seen[st] = true
        al.add(st)
        while (f) {
            f = false
            for (a in als[st]) {
                if (!seen[a] && cs[a] <= 3) {
                    seen[a] = true
                    al.add(a)
                    if (cs[a] == 3) {
                        f = true
                        st = a
                    }
                    break
                }
            }
        }
        res = Array<IntArray>(n / al.size) { IntArray(al.size) }
        for (i in res[0].indices) {
            res[0][i] = al[i]!!
        }
        for (i in 1 until res.size) {
            for (j in res[0].indices) {
                for (a in als[res[i - 1][j]]) {
                    if (!seen[a]) {
                        res[i][j] = a
                        seen[a] = true
                        break
                    }
                }
            }
        }
        return res
    }

    private fun getInts2(
        n: Int,
        st: Int,
        row2: Int,
        seen: BooleanArray,
        als: Array<ArrayList<Int>>,
    ): Array<IntArray> {
        var res: Array<IntArray> = Array<IntArray>(2) { IntArray(n / 2) }
        res[0][0] = st
        res[1][0] = row2
        seen[row2] = true
        seen[st] = seen[row2]
        for (i in 1 until res[0].size) {
            for (a in als[res[0][i - 1]]) {
                if (!seen[a]) {
                    res[0][i] = a
                    seen[a] = true
                    break
                }
            }
            for (a in als[res[1][i - 1]]) {
                if (!seen[a]) {
                    res[1][i] = a
                    seen[a] = true
                    break
                }
            }
        }
        return res
    }
}
```