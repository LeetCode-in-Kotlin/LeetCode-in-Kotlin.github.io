[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1584\. Min Cost to Connect All Points

Medium

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

The cost of connecting two points <code>[x<sub>i</sub>, y<sub>i</sub>]</code> and <code>[x<sub>j</sub>, y<sub>j</sub>]</code> is the **manhattan distance** between them: <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>, where `|val|` denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = \[\[0,0],[2,2],[3,10],[5,2],[7,0]]

**Output:** 20

**Explanation:** ![](https://assets.leetcode.com/uploads/2020/08/26/c.png)

We can connect the points as shown above to get the minimum cost of 20.

Notice that there is a unique path between every pair of points.

**Example 2:**

**Input:** points = \[\[3,12],[-2,5],[-4,1]]

**Output:** 18

**Constraints:**

*   `1 <= points.length <= 1000`
*   <code>-10<sup>6</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>6</sup></code>
*   All pairs <code>(x<sub>i</sub>, y<sub>i</sub>)</code> are distinct.

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun minCostConnectPoints(points: Array<IntArray>): Int {
        val v = points.size
        if (v == 2) {
            return getDistance(points[0], points[1])
        }
        val pq = PriorityQueue(v, Pair())
        val mst = BooleanArray(v)
        val dist = IntArray(v)
        val parent = IntArray(v)
        dist.fill(1000000)
        parent.fill(-1)
        dist[0] = 0
        parent[0] = 0
        for (i in 0 until v) {
            pq.add(Pair(dist[i], i))
        }
        constructMST(parent, points, mst, pq, dist)
        var cost = 0
        for (i in 1 until parent.size) {
            cost += getDistance(points[parent[i]], points[i])
        }
        return cost
    }

    private fun constructMST(
        parent: IntArray,
        points: Array<IntArray>,
        mst: BooleanArray,
        pq: PriorityQueue<Pair>,
        dist: IntArray,
    ) {
        if (!containsFalse(mst)) {
            return
        }
        val newPair = pq.poll()
        val pointIndex: Int = newPair.v
        mst[pointIndex] = true
        for (i in parent.indices) {
            val d = getDistance(points[pointIndex], points[i])
            if (!mst[i] && d < dist[i]) {
                dist[i] = d
                pq.add(Pair(dist[i], i))
                parent[i] = pointIndex
            }
        }
        constructMST(parent, points, mst, pq, dist)
    }

    private fun containsFalse(mst: BooleanArray): Boolean {
        for (b in mst) {
            if (!b) {
                return true
            }
        }
        return false
    }

    private fun getDistance(p1: IntArray, p2: IntArray): Int {
        return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1])
    }

    class Pair : Comparator<Pair> {
        var dis = 0
        var v = 0

        constructor()
        constructor(dis: Int, v: Int) {
            this.dis = dis
            this.v = v
        }

        override fun compare(p1: Pair, p2: Pair): Int {
            return p1.dis - p2.dis
        }
    }
}
```