[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1637\. Widest Vertical Area Between Two Points Containing No Points

Medium

Given `n` `points` on a 2D plane where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>, Return_ the **widest vertical area** between two points such that no points are inside the area._

A **vertical area** is an area of fixed-width extending infinitely along the y-axis (i.e., infinite height). The **widest vertical area** is the one with the maximum width.

Note that points **on the edge** of a vertical area **are not** considered included in the area.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/19/points3.png)

**Input:** points = \[\[8,7],[9,9],[7,4],[9,7]]

**Output:** 1

**Explanation:** Both the red and the blue area are optimal.

**Example 2:**

**Input:** points = \[\[3,1],[9,0],[1,0],[1,4],[5,3],[8,8]]

**Output:** 3

**Constraints:**

*   `n == points.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maxWidthOfVerticalArea(points: Array<IntArray>): Int {
        val xValues = IntArray(points.size)
        for (i in points.indices) {
            xValues[i] = points[i][0]
        }
        xValues.sort()
        var max = 0
        for (j in 0 until xValues.size - 1) {
            if (xValues[j + 1] - xValues[j] > max) {
                max = xValues[j + 1] - xValues[j]
            }
        }
        return max
    }
}
```