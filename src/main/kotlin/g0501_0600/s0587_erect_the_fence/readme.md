[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 587\. Erect the Fence

Hard

You are given an array `trees` where <code>trees[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the location of a tree in the garden.

Fence the entire garden using the minimum length of rope, as it is expensive. The garden is well-fenced only if **all the trees are enclosed**.

Return _the coordinates of trees that are exactly located on the fence perimeter_. You may return the answer in **any order**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/erect2-plane.jpg)

**Input:** trees = \[\[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]

**Output:** [[1,1],[2,0],[4,2],[3,3],[2,4]]

**Explanation:** All the trees will be on the perimeter of the fence except the tree at [2, 2], which will be inside the fence.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/erect1-plane.jpg)

**Input:** trees = \[\[1,2],[2,2],[4,2]]

**Output:** [[4,2],[2,2],[1,2]]

**Explanation:** The fence forms a line that passes through all the trees.

**Constraints:**

*   `1 <= trees.length <= 3000`
*   `trees[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>
*   All the given positions are **unique**.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.atan2
import kotlin.math.pow
import kotlin.math.sqrt

class Solution {
    private fun dist(p1: Pair<Int, Int>, p2: Pair<Int, Int>): Double {
        return sqrt((p2.second - p1.second).toDouble().pow(2.0) + Math.pow((p2.first - p1.first).toDouble(), 2.0))
    }

    private fun angle(p1: Pair<Int, Int>, p2: Pair<Int, Int>): Double {
        return atan2((p2.second - p1.second).toDouble(), (p2.first - p1.first).toDouble()).let {
            if (it < 0) return (2.0 * Math.PI + it) else it
        }
    }

    fun outerTrees(trees: Array<IntArray>): Array<IntArray> {
        if (trees.size < 3) {
            return trees
        }
        val left = trees.asSequence().map { it[0] to it[1] }.toMutableList()
        left.sortWith(compareBy<Pair<Int, Int>> { it.second }.thenBy { it.first })
        val firstPoint = left[0]
        var nowPoint = firstPoint
        val pointList = mutableListOf(nowPoint)
        var prevAngle = 0.0
        while (true) {
            val nowList = mutableListOf<Pair<Pair<Int, Int>, Double>>()
            var nowMinAngleDiff = 7.0
            var nowMinAngle = 7.0
            left.forEach {
                if (it != nowPoint) {
                    val angle = angle(nowPoint, it)
                    if (abs(angle - nowMinAngle) < 0.0000001) {
                        nowList.add(it to dist(it, nowPoint))
                    } else {
                        val diff = if (angle >= prevAngle) (angle - prevAngle) else 2.0 * Math.PI - (angle - prevAngle)
                        if ((diff) < nowMinAngleDiff) {
                            nowMinAngle = angle
                            nowMinAngleDiff = (diff)
                            nowList.clear()
                            nowList.add(it to dist(it, nowPoint))
                        }
                    }
                }
            }
            prevAngle = nowMinAngle
            nowList.sortBy { it.second }
            val nowListOnlyPoints = nowList.map { it.first }.toMutableList()
            if (nowListOnlyPoints.last() == firstPoint) {
                nowListOnlyPoints.removeAt(nowListOnlyPoints.size - 1)
                left.removeAll(nowListOnlyPoints)
                pointList.addAll(nowListOnlyPoints)
                break
            } else {
                nowPoint = nowListOnlyPoints.last()
                left.removeAll(nowListOnlyPoints)
                pointList.addAll(nowListOnlyPoints)
            }
        }
        return pointList.asSequence().map { intArrayOf(it.first, it.second) }.toList().toTypedArray()
    }
}
```