[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1401\. Circle and Rectangle Overlapping

Medium

You are given a circle represented as `(radius, xCenter, yCenter)` and an axis-aligned rectangle represented as `(x1, y1, x2, y2)`, where `(x1, y1)` are the coordinates of the bottom-left corner, and `(x2, y2)` are the coordinates of the top-right corner of the rectangle.

Return `true` _if the circle and rectangle are overlapped otherwise return_ `false`. In other words, check if there is **any** point <code>(x<sub>i</sub>, y<sub>i</sub>)</code> that belongs to the circle and the rectangle at the same time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/02/20/sample_4_1728.png)

**Input:** radius = 1, xCenter = 0, yCenter = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1

**Output:** true

**Explanation:** Circle and rectangle share the point (1,0).

**Example 2:**

**Input:** radius = 1, xCenter = 1, yCenter = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1

**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/02/20/sample_2_1728.png)

**Input:** radius = 1, xCenter = 0, yCenter = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1

**Output:** true

**Constraints:**

*   `1 <= radius <= 2000`
*   <code>-10<sup>4</sup> <= xCenter, yCenter <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= x1 < x2 <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= y1 < y2 <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun checkOverlap(
        radius: Int,
        xCenter: Int,
        yCenter: Int,
        x1: Int,
        y1: Int,
        x2: Int,
        y2: Int
    ): Boolean {
        // Find the closest point to the circle within the rectangle
        val closestX = clamp(xCenter, x1, x2)
        val closestY = clamp(yCenter, y1, y2)
        // Calculate the distance between the circle's center and this closest point
        val distanceX = xCenter - closestX
        val distanceY = yCenter - closestY
        // If the distance is less than the circle's radius, an intersection occurs
        val distanceSquared = distanceX * distanceX + distanceY * distanceY
        return distanceSquared <= radius * radius
    }

    private fun clamp(`val`: Int, min: Int, max: Int): Int {
        return Math.max(min, Math.min(max, `val`))
    }
}
```