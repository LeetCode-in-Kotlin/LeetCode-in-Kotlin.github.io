[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1697\. Checking Existence of Edge Length Limited Paths

Hard

An undirected graph of `n` nodes is defined by `edgeList`, where <code>edgeList[i] = [u<sub>i</sub>, v<sub>i</sub>, dis<sub>i</sub>]</code> denotes an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with distance <code>dis<sub>i</sub></code>. Note that there may be **multiple** edges between two nodes.

Given an array `queries`, where <code>queries[j] = [p<sub>j</sub>, q<sub>j</sub>, limit<sub>j</sub>]</code>, your task is to determine for each `queries[j]` whether there is a path between <code>p<sub>j</sub></code> and <code>q<sub>j</sub></code> such that each edge on the path has a distance **strictly less than** <code>limit<sub>j</sub></code> .

Return _a **boolean array**_ `answer`_, where_ `answer.length == queries.length` _and the_ <code>j<sup>th</sup></code> _value of_ `answer` _is_ `true` _if there is a path for_ `queries[j]` _is_ `true`_, and_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/08/h.png)

**Input:** n = 3, edgeList = \[\[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = \[\[0,1,2],[0,2,5]]

**Output:** [false,true]

**Explanation:** The above figure shows the given graph. Note that there are two overlapping edges between 0 and 1 with distances 2 and 16.

For the first query, between 0 and 1 there is no path where each distance is less than 2, thus we return false for this query.

For the second query, there is a path (0 -> 1 -> 2) of two edges with distances less than 5, thus we return true for this query.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/08/q.png)

**Input:** n = 5, edgeList = \[\[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = \[\[0,4,14],[1,4,13]]

**Output:** [true,false] **Exaplanation:** The above figure shows the given graph.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= edgeList.length, queries.length <= 10<sup>5</sup></code>
*   `edgeList[i].length == 3`
*   `queries[j].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub>, p<sub>j</sub>, q<sub>j</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>p<sub>j</sub> != q<sub>j</sub></code>
*   <code>1 <= dis<sub>i</sub>, limit<sub>j</sub> <= 10<sup>9</sup></code>
*   There may be **multiple** edges between two nodes.

## Solution

```kotlin
class Solution {
    private class Dsu(n: Int) {
        private val parent: IntArray

        init {
            parent = IntArray(n)
            parent.fill(-1)
        }

        fun find(num: Int): Int {
            if (parent[num] == -1) {
                return num
            }
            parent[num] = find(parent[num])
            return parent[num]
        }

        fun union(a: Int, b: Int) {
            val p1 = find(a)
            val p2 = find(b)
            if (p1 != p2) {
                parent[p2] = p1
            }
        }
    }

    fun distanceLimitedPathsExist(n: Int, edgeList: Array<IntArray>, queries: Array<IntArray>): BooleanArray {
        edgeList.sortWith { o1: IntArray, o2: IntArray -> Integer.compare(o1[2], o2[2]) }
        val data = Array(queries.size) { IntArray(4) }
        for (i in queries.indices) {
            data[i] = intArrayOf(queries[i][0], queries[i][1], queries[i][2], i)
        }
        data.sortWith { o1: IntArray, o2: IntArray -> Integer.compare(o1[2], o2[2]) }
        val d = Dsu(n)
        var j = 0
        val ans = BooleanArray(queries.size)
        for (datum in data) {
            while (j < edgeList.size && edgeList[j][2] < datum[2]) {
                d.union(edgeList[j][0], edgeList[j][1])
                j++
            }
            if (d.find(datum[0]) == d.find(datum[1])) {
                ans[datum[3]] = true
            }
        }
        return ans
    }
}
```