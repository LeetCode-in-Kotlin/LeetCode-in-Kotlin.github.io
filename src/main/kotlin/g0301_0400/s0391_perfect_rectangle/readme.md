[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 391\. Perfect Rectangle

Hard

Given an array `rectangles` where <code>rectangles[i] = [x<sub>i</sub>, y<sub>i</sub>, a<sub>i</sub>, b<sub>i</sub>]</code> represents an axis-aligned rectangle. The bottom-left point of the rectangle is <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and the top-right point of it is <code>(a<sub>i</sub>, b<sub>i</sub>)</code>.

Return `true` _if all the rectangles together form an exact cover of a rectangular region_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg)

**Input:** rectangles = \[\[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]

**Output:** true

**Explanation:** All 5 rectangles together form an exact cover of a rectangular region.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg)

**Input:** rectangles = \[\[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]

**Output:** false

**Explanation:** Because there is a gap between the two rectangular regions.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)

**Input:** rectangles = \[\[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]

**Output:** false

**Explanation:** Because two of the rectangles overlap with each other.

**Constraints:**

*   <code>1 <= rectangles.length <= 2 * 10<sup>4</sup></code>
*   `rectangles[i].length == 4`
*   <code>-10<sup>5</sup> <= x<sub>i</sub>, y<sub>i</sub>, a<sub>i</sub>, b<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.Objects

class Solution {
    fun isRectangleCover(rectangles: Array<IntArray>): Boolean {
        val container: MutableSet<Point> = HashSet()
        // add each rectangle area to totalArea
        var totalArea = 0
        // A rectangle has four points, if a point appears twice, it will be deleted it from the set
        for (rectangle in rectangles) {
            totalArea += (rectangle[2] - rectangle[0]) * (rectangle[3] - rectangle[1])
            val p1 = Point(rectangle[0], rectangle[1])
            val p2 = Point(rectangle[2], rectangle[1])
            val p3 = Point(rectangle[2], rectangle[3])
            val p4 = Point(rectangle[0], rectangle[3])
            if (container.contains(p1)) {
                container.remove(p1)
            } else {
                container.add(p1)
            }
            if (container.contains(p2)) {
                container.remove(p2)
            } else {
                container.add(p2)
            }
            if (container.contains(p3)) {
                container.remove(p3)
            } else {
                container.add(p3)
            }
            if (container.contains(p4)) {
                container.remove(p4)
            } else {
                container.add(p4)
            }
        }
        // A perfect rectangle must has four points
        if (container.size != 4) {
            return false
        }

        // these four points represent the last perfect rectangle, check this rectangle area to the
        // totalArea
        var minX = Int.MAX_VALUE
        var maxX = Int.MIN_VALUE
        var minY = Int.MAX_VALUE
        var maxY = Int.MIN_VALUE
        for (p in container) {
            minX = Math.min(minX, p.x)
            maxX = Math.max(maxX, p.x)
            minY = Math.min(minY, p.y)
            maxY = Math.max(maxY, p.y)
        }
        return totalArea == (maxX - minX) * (maxY - minY)
    }

    private class Point(val x: Int, val y: Int) {
        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other == null || javaClass != other.javaClass) {
                return false
            }
            val point = other as Point
            return x == point.x && y == point.y
        }

        override fun hashCode(): Int {
            return Objects.hash(x, y)
        }
    }
}
```