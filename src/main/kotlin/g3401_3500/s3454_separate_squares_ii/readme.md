[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3454\. Separate Squares II

Hard

You are given a 2D integer array `squares`. Each <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code> represents the coordinates of the bottom-left point and the side length of a square parallel to the x-axis.

Find the **minimum** y-coordinate value of a horizontal line such that the total area covered by squares above the line _equals_ the total area covered by squares below the line.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Note**: Squares **may** overlap. Overlapping areas should be counted **only once** in this version.

**Example 1:**

**Input:** squares = \[\[0,0,1],[2,2,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4065example1drawio.png)

Any horizontal line between `y = 1` and `y = 2` results in an equal split, with 1 square unit above and 1 square unit below. The minimum y-value is 1.

**Example 2:**

**Input:** squares = \[\[0,0,2],[1,1,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4065example2drawio.png)

Since the blue square overlaps with the red square, it will not be counted again. Thus, the line `y = 1` splits the squares into two equal parts.

**Constraints:**

*   <code>1 <= squares.length <= 5 * 10<sup>4</sup></code>
*   <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code>
*   `squares[i].length == 3`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= l<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun separateSquares(squares: Array<IntArray>): Double {
        val n = squares.size
        val m = 2 * n
        val events = createEvents(squares, m)
        val xsRaw = createXsRaw(squares, m)
        events.sortWith<Event> { a, b: Event -> a.y.compareTo(b.y) }
        val xs = compress(xsRaw)
        val totalUnionArea = calculateTotalUnionArea(events, xs, m)
        val target = totalUnionArea / 2.0
        return findSplitPoint(events, xs, m, target)
    }

    private fun createEvents(squares: Array<IntArray>, m: Int): Array<Event> {
        val events = Array(m) { Event(0.0, 0.0, 0.0, 0) }
        var idx = 0
        for (sq in squares) {
            val x = sq[0].toDouble()
            val y = sq[1].toDouble()
            val l = sq[2].toDouble()
            val x2 = x + l
            val y2 = y + l
            events[idx++] = Event(y, x, x2, 1)
            events[idx++] = Event(y2, x, x2, -1)
        }
        return events
    }

    private fun createXsRaw(squares: Array<IntArray>, m: Int): DoubleArray {
        val xsRaw = DoubleArray(m)
        var xIdx = 0
        for (sq in squares) {
            val x = sq[0].toDouble()
            val l = sq[2].toDouble()
            xsRaw[xIdx++] = x
            xsRaw[xIdx++] = x + l
        }
        return xsRaw
    }

    private fun calculateTotalUnionArea(events: Array<Event>, xs: DoubleArray, m: Int): Double {
        val segTree = SegmentTree(xs)
        var totalUnionArea = 0.0
        var lastY = events[0].y
        var i = 0
        while (i < m) {
            val curY = events[i].y
            if (curY > lastY) {
                val unionX = segTree.query()
                totalUnionArea += unionX * (curY - lastY)
                lastY = curY
            }
            while (i < m && events[i].y == curY) {
                val indices = findIndices(xs, events[i])
                segTree.update(1, 0, xs.size - 1, indices[0], indices[1], events[i].type)
                i++
            }
        }
        return totalUnionArea
    }

    private fun findSplitPoint(events: Array<Event>, xs: DoubleArray, m: Int, target: Double): Double {
        val segTree = SegmentTree(xs)
        var lastY = events[0].y
        var cumArea = 0.0
        var i = 0
        while (i < m) {
            val curY = events[i].y
            if (curY > lastY) {
                val unionX = segTree.query()
                val dy = curY - lastY
                if (cumArea + unionX * dy >= target - 1e-10) {
                    return lastY + (target - cumArea) / unionX
                }
                cumArea += unionX * dy
                lastY = curY
            }
            while (i < m && events[i].y == curY) {
                val indices = findIndices(xs, events[i])
                segTree.update(1, 0, xs.size - 1, indices[0], indices[1], events[i].type)
                i++
            }
        }
        return lastY
    }

    private fun findIndices(xs: DoubleArray, event: Event): IntArray {
        var lIdx = xs.binarySearch(event.x1)
        if (lIdx < 0) {
            lIdx = -lIdx - 1
        }
        var rIdx = xs.binarySearch(event.x2)
        if (rIdx < 0) {
            rIdx = -rIdx - 1
        }
        return intArrayOf(lIdx, rIdx)
    }

    private fun compress(arr: DoubleArray): DoubleArray {
        arr.sort()
        var cnt = 1
        for (i in 1..<arr.size) {
            if (arr[i] != arr[i - 1]) {
                cnt++
            }
        }
        val res = DoubleArray(cnt)
        res[0] = arr[0]
        var j = 1
        for (i in 1..<arr.size) {
            if (arr[i] != arr[i - 1]) {
                res[j++] = arr[i]
            }
        }
        return res
    }

    private class Event(var y: Double, var x1: Double, var x2: Double, var type: Int)

    private class SegmentTree(var xs: DoubleArray) {
        var n: Int
        var tree: DoubleArray
        var count: IntArray

        init {
            this.n = xs.size
            tree = DoubleArray(4 * n)
            count = IntArray(4 * n)
        }

        fun update(idx: Int, l: Int, r: Int, ql: Int, qr: Int, `val`: Int) {
            if (qr <= l || ql >= r) {
                return
            }
            if (ql <= l && r <= qr) {
                count[idx] += `val`
            } else {
                val mid = (l + r) shr 1
                update(idx shl 1, l, mid, ql, qr, `val`)
                update(idx shl 1 or 1, mid, r, ql, qr, `val`)
            }
            if (count[idx] > 0) {
                tree[idx] = xs[r] - xs[l]
            } else if (r - l == 1) {
                tree[idx] = 0.0
            } else {
                tree[idx] = tree[idx shl 1] + tree[idx shl 1 or 1]
            }
        }

        fun query(): Double {
            return tree[1]
        }
    }
}
```