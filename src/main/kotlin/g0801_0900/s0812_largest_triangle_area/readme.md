[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 812\. Largest Triangle Area

Easy

Given an array of points on the **X-Y** plane `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>, return _the area of the largest triangle that can be formed by any three different points_. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png)

**Input:** points = \[\[0,0],[0,1],[1,0],[0,2],[2,0]]

**Output:** 2.00000

**Explanation:** The five points are shown in the above figure. The red triangle is the largest.

**Example 2:**

**Input:** points = \[\[1,0],[0,0],[0,1]]

**Output:** 0.50000

**Constraints:**

*   `3 <= points.length <= 50`
*   <code>-50 <= x<sub>i</sub>, y<sub>i</sub> <= 50</code>
*   All the given points are **unique**.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun largestTriangleArea(points: Array<IntArray>): Double {
        val n = points.size
        var max = 0.0
        for (i in 0 until n) {
            for (j in i + 1 until n) {
                for (k in j + 1 until n) {
                    var area: Double
                    val a = points[i]
                    val b = points[j]
                    val c = points[k]
                    area = abs(area(a, b) + area(b, c) + area(c, a))
                    if (area > max) {
                        max = area
                    }
                }
            }
        }
        return max
    }

    private fun area(a: IntArray, b: IntArray): Double {
        val l = b[0] - a[0]
        val h = (a[1] + b[1] + 200) / 2.0
        return l * h
    }
}
```