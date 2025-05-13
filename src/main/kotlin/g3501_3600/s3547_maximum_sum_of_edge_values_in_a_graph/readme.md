[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3547\. Maximum Sum of Edge Values in a Graph

Hard

You are given an **und****irected** graph of `n` nodes, numbered from `0` to `n - 1`. Each node is connected to **at most** 2 other nodes.

The graph consists of `m` edges, represented by a 2D array `edges`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>.

You have to assign a **unique** value from `1` to `n` to each node. The value of an edge will be the **product** of the values assigned to the two nodes it connects.

Your score is the sum of the values of all edges in the graph.

Return the **maximum** score you can achieve.

**Example 1:**

![](https://assets.leetcode.com/uploads/2025/03/23/graphproblemex1drawio.png)

**Input:** n = 7, edges = \[\[0,1],[1,2],[2,0],[3,4],[4,5],[5,6]]

**Output:** 130

**Explanation:**

The diagram above illustrates an optimal assignment of values to nodes. The sum of the values of the edges is: `(7 * 6) + (7 * 5) + (6 * 5) + (1 * 3) + (3 * 4) + (4 * 2) = 130`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2025/03/23/graphproblemex2drawio.png)

**Input:** n = 6, edges = \[\[0,3],[4,5],[2,0],[1,3],[2,4],[1,5]]

**Output:** 82

**Explanation:**

The diagram above illustrates an optimal assignment of values to nodes. The sum of the values of the edges is: `(1 * 2) + (2 * 4) + (4 * 6) + (6 * 5) + (5 * 3) + (3 * 1) = 82`.

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   `m == edges.length`
*   `1 <= m <= n`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.
*   Each node is connected to at most 2 other nodes.

## Solution

```kotlin
class Solution {
    private lateinit var p: IntArray
    private lateinit var c: BooleanArray
    private lateinit var s: IntArray

    fun maxScore(n: Int, edges: Array<IntArray>): Long {
        initializeArrays(n)
        processEdges(edges)
        val circles: MutableList<Int> = ArrayList<Int>()
        val chains: MutableList<Int> = ArrayList<Int>()
        findParentsAndUpdateCircles()
        collectCirclesAndChains(circles, chains)
        circles.sort()
        chains.sortWith { a: Int, b: Int -> Integer.compare(b, a) }
        return calculateScore(n, circles, chains)
    }

    private fun initializeArrays(n: Int) {
        p = IntArray(n)
        c = BooleanArray(n)
        s = IntArray(n)
        for (i in 0..<n) {
            p[i] = i
            s[i] = 1
        }
    }

    private fun processEdges(edges: Array<IntArray>) {
        for (ele in edges) {
            join(ele[0], ele[1])
        }
    }

    private fun findParentsAndUpdateCircles() {
        for (i in p.indices) {
            p[i] = findParent(i)
            if (c[i]) {
                c[p[i]] = true
            }
        }
    }

    private fun collectCirclesAndChains(circles: MutableList<Int>, chains: MutableList<Int>) {
        for (i in p.indices) {
            if (p[i] == i) {
                val size = s[i]
                if (c[i]) {
                    circles.add(size)
                } else {
                    chains.add(size)
                }
            }
        }
    }

    private fun calculateScore(n: Int, circles: MutableList<Int>, chains: MutableList<Int>): Long {
        var ret: Long = 0
        var start = n
        ret += processCircles(circles, start)
        start = n - getTotalCircleSize(circles)
        ret += processChains(chains, start)
        return ret
    }

    private fun getTotalCircleSize(circles: MutableList<Int>): Int {
        return circles.stream().mapToInt { obj: Int -> obj }.sum()
    }

    private fun processCircles(circles: MutableList<Int>, start: Int): Long {
        var start = start
        var ret: Long = 0
        for (size in circles) {
            if (size == 1) {
                continue
            }
            val temp = createTempArray(size, start)
            val pro = calculateProduct(temp, true)
            ret += pro
            start = start - size
        }
        return ret
    }

    private fun processChains(chains: MutableList<Int>, start: Int): Long {
        var start = start
        var ret: Long = 0
        for (size in chains) {
            if (size == 1) {
                continue
            }
            val temp = createTempArray(size, start)
            val pro = calculateProduct(temp, false)
            ret += pro
            start = start - size
        }
        return ret
    }

    private fun createTempArray(size: Int, start: Int): IntArray {
        val temp = IntArray(size)
        var ptr1 = 0
        var ptr2 = size - 1
        val curStart = start - size + 1
        for (i in 0..<size) {
            if (i % 2 == 0) {
                temp[ptr1++] = curStart + i
            } else {
                temp[ptr2--] = curStart + i
            }
        }
        return temp
    }

    private fun calculateProduct(temp: IntArray, isCircle: Boolean): Long {
        var pro: Long = 0
        for (i in 1..<temp.size) {
            pro += temp[i].toLong() * temp[i - 1]
        }
        if (isCircle) {
            pro += temp[0].toLong() * temp[temp.size - 1]
        }
        return pro
    }

    private fun findParent(x: Int): Int {
        if (p[x] != x) {
            p[x] = findParent(p[x])
        }
        return p[x]
    }

    private fun join(a: Int, b: Int) {
        val bp = findParent(a)
        val ap = findParent(b)
        if (bp == ap) {
            c[bp] = true
            return
        }
        val s1 = s[ap]
        val s2 = s[bp]
        if (s1 > s2) {
            p[bp] = ap
            s[ap] += s[bp]
        } else {
            p[ap] = bp
            s[bp] += s[ap]
        }
    }
}
```