[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 939\. Minimum Area Rectangle

Medium

You are given an array of points in the **X-Y** plane `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

Return _the minimum area of a rectangle formed from these points, with sides parallel to the X and Y axes_. If there is not any such rectangle, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG)

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[2,2]]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG)

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]

**Output:** 2

**Constraints:**

*   `1 <= points.length <= 500`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 4 * 10<sup>4</sup></code>
*   All the given points are **unique**.

## Solution

```kotlin
import java.util.Arrays
import kotlin.math.abs

class Solution {
    fun minAreaRect(points: Array<IntArray>): Int {
        if (points.size < 4) {
            return 0
        }
        val map: MutableMap<Int, MutableSet<Int>> = HashMap()
        for (p in points) {
            map.putIfAbsent(p[0], HashSet())
            map.getValue(p[0]).add(p[1])
        }
        Arrays.sort(
            points
        ) { a: IntArray, b: IntArray ->
            if (a[0] == b[0]) Integer.compare(
                a[1],
                b[1]
            ) else Integer.compare(a[0], b[0])
        }
        var min = Int.MAX_VALUE
        for (i in 0 until points.size - 2) {
            for (j in i + 1 until points.size - 1) {
                val p1 = points[i]
                val p2 = points[j]
                val area = abs((p1[0] - p2[0]) * (p1[1] - p2[1]))
                if (area >= min || area == 0) {
                    continue
                }
                if (map.getValue(p1[0]).contains(p2[1]) && map.getValue(p2[0]).contains(p1[1])) {
                    min = area
                }
            }
        }
        return if (min == Int.MAX_VALUE) 0 else min
    }
}
```