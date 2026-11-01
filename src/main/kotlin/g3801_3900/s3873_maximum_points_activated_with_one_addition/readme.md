[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3873\. Maximum Points Activated with One Addition

Hard

You are given a 2D integer array `points`, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinates of the <code>i<sup>th</sup></code> point. All coordinates in `points` are **distinct**.

If a point is **activated**, then all points that have the **same** x-coordinate **or** y-coordinate become **activated** as well.

Activation continues until no additional points can be activated.

You may add **one additional** point at any integer coordinate `(x, y)` not already present in `points`. Activation begins by **activating** this **newly added point**.

Return an integer denoting the **maximum** number of points that can be activated, including the newly added point.

**Example 1:**

**Input:** points = \[\[1,1],[1,2],[2,2]]

**Output:** 4

**Explanation:**

Adding and activating a point such as `(1, 3)` causes activations:

*   `(1, 3)` shares `x = 1` with `(1, 1)` and `(1, 2)` -> `(1, 1)` and `(1, 2)` become activated.
*   `(1, 2)` shares `y = 2` with `(2, 2)` -> `(2, 2)` becomes activated.

Thus, the activated points are `(1, 3)`, `(1, 1)`, `(1, 2)`, `(2, 2)`, so 4 points in total. We can show this is the maximum activated.

**Example 2:**

**Input:** points = \[\[2,2],[1,1],[3,3]]

**Output:** 3

**Explanation:**

Adding and activating a point such as `(1, 2)` causes activations:

*   `(1, 2)` shares `x = 1` with `(1, 1)` -> `(1, 1)` becomes activated.
*   `(1, 2)` shares `y = 2` with `(2, 2)` -> `(2, 2)` becomes activated.

Thus, the activated points are `(1, 2)`, `(1, 1)`, `(2, 2)`, so 3 points in total. We can show this is the maximum activated.

**Example 3:**

**Input:** points = \[\[2,3],[2,2],[1,1],[4,5]]

**Output:** 4

**Explanation:**

Adding and activating a point such as `(2, 1)` causes activations:

*   `(2, 1)` shares `x = 2` with `(2, 3)` and `(2, 2)` -> `(2, 3)` and `(2, 2)` become activated.
*   `(2, 1)` shares `y = 1` with `(1, 1)` -> `(1, 1)` becomes activated.

Thus, the activated points are `(2, 1)`, `(2, 3)`, `(2, 2)`, `(1, 1)`, so 4 points in total.

**Constraints:**

*   <code>1 <= points.length <= 10<sup>5</sup></code>
*   <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>
*   <code>-10<sup>9</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>
*   `points` contains all **distinct** coordinates.

## Solution

```kotlin
class Solution {
    fun maxActivated(points: Array<IntArray>): Int = maxEnergized(points)

    private fun maxEnergized(relays: Array<IntArray>): Int {
        val n = relays.size
        val parent = IntArray(n)
        val size = IntArray(n)
        for (i in 0..<n) {
            parent[i] = i
            size[i] = 1
        }
        val firstInCol: MutableMap<Int, Int> = HashMap<Int, Int>(n * 2)
        val firstInRow: MutableMap<Int, Int> = HashMap<Int, Int>(n * 2)
        for (i in 0..<n) {
            val col = relays[i][0]
            val row = relays[i][1]
            val c = firstInCol.putIfAbsent(col, i)
            if (c != null) {
                union(parent, size, c, i)
            }
            val r = firstInRow.putIfAbsent(row, i)
            if (r != null) {
                union(parent, size, r, i)
            }
        }
        var best1 = 0
        var best2 = 0
        for (i in 0..<n) {
            if (find(parent, i) == i) {
                val s = size[i]
                if (s >= best1) {
                    best2 = best1
                    best1 = s
                } else if (s > best2) {
                    best2 = s
                }
            }
        }
        return best1 + best2 + 1
    }

    private fun find(parent: IntArray, x: Int): Int {
        var x = x
        var root = x
        while (parent[root] != root) {
            root = parent[root]
        }
        while (parent[x] != root) {
            val next = parent[x]
            parent[x] = root
            x = next
        }
        return root
    }

    private fun union(parent: IntArray, size: IntArray, a: Int, b: Int) {
        var ra = find(parent, a)
        var rb = find(parent, b)
        if (ra == rb) {
            return
        }
        if (size[ra] < size[rb]) {
            val t = ra
            ra = rb
            rb = t
        }
        parent[rb] = ra
        size[ra] += size[rb]
    }
}
```