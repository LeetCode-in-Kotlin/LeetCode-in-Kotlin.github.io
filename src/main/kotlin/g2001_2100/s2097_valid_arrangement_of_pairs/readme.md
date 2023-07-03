[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2097\. Valid Arrangement of Pairs

Hard

You are given a **0-indexed** 2D integer array `pairs` where <code>pairs[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. An arrangement of `pairs` is **valid** if for every index `i` where `1 <= i < pairs.length`, we have <code>end<sub>i-1</sub> == start<sub>i</sub></code>.

Return _**any** valid arrangement of_ `pairs`.

**Note:** The inputs will be generated such that there exists a valid arrangement of `pairs`.

**Example 1:**

**Input:** pairs = \[\[5,1],[4,5],[11,9],[9,4]]

**Output:** [[11,9],[9,4],[4,5],[5,1]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 9 == 9 = start<sub>1</sub>

end<sub>1</sub> = 4 == 4 = start<sub>2</sub>

end<sub>2</sub> = 5 == 5 = start<sub>3</sub>

**Example 2:**

**Input:** pairs = \[\[1,3],[3,2],[2,1]]

**Output:** [[1,3],[3,2],[2,1]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 3 == 3 = start<sub>1</sub>

end<sub>1</sub> = 2 == 2 = start<sub>2</sub>

The arrangements [[2,1],[1,3],[3,2]] and [[3,2],[2,1],[1,3]] are also valid.

**Example 3:**

**Input:** pairs = \[\[1,2],[1,3],[2,1]]

**Output:** [[1,2],[2,1],[1,3]]

**Explanation:**

This is a valid arrangement since end<sub>i-1</sub> always equals start<sub>i</sub>.

end<sub>0</sub> = 2 == 2 = start<sub>1</sub>

end<sub>1</sub> = 1 == 1 = start<sub>2</sub>

**Constraints:**

*   <code>1 <= pairs.length <= 10<sup>5</sup></code>
*   `pairs[i].length == 2`
*   <code>0 <= start<sub>i</sub>, end<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>start<sub>i</sub> != end<sub>i</sub></code>
*   No two pairs are exactly the same.
*   There **exists** a valid arrangement of `pairs`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

@Suppress("NAME_SHADOWING")
class Solution {
    fun validArrangement(pairs: Array<IntArray>): Array<IntArray> {
        val inOutedge = HashMap<Int, IntArray>()
        val adList = getAdList(pairs, inOutedge)
        val start = getStart(inOutedge)
        val res = Array(pairs.size) { IntArray(2) }
        getRes(start, adList, res, pairs.size - 1)
        return res
    }

    private fun getAdList(
        pairs: Array<IntArray>,
        inOutEdge: HashMap<Int, IntArray>
    ): HashMap<Int, Queue<Int>> {
        val adList = HashMap<Int, Queue<Int>>()
        for (pair in pairs) {
            val s = pair[0]
            val d = pair[1]
            val set = adList.computeIfAbsent(s) { _: Int? -> LinkedList() }
            set.add(d)
            val sEdgeCnt = inOutEdge.computeIfAbsent(s) { _: Int? -> IntArray(2) }
            val dEdgeCnt = inOutEdge.computeIfAbsent(d) { _: Int? -> IntArray(2) }
            sEdgeCnt[1]++
            dEdgeCnt[0]++
        }
        return adList
    }

    private fun getRes(k: Int, adList: HashMap<Int, Queue<Int>>, res: Array<IntArray>, idx: Int): Int {
        var idx = idx
        val edges = adList[k] ?: return idx
        while (edges.isNotEmpty()) {
            val edge = edges.poll()
            idx = getRes(edge, adList, res, idx)
            res[idx--] = intArrayOf(k, edge)
        }
        return idx
    }

    private fun getStart(map: HashMap<Int, IntArray>): Int {
        var start = -1
        for ((k, value) in map) {
            val inEdge = value[0]
            val outEdge = value[1]
            start = k
            if (outEdge - inEdge == 1) {
                return k
            }
        }
        return start
    }
}
```