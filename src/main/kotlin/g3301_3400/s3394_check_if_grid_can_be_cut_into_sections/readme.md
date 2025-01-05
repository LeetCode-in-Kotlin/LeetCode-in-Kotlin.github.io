[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3394\. Check if Grid can be Cut into Sections

Medium

You are given an integer `n` representing the dimensions of an `n x n` grid, with the origin at the bottom-left corner of the grid. You are also given a 2D array of coordinates `rectangles`, where `rectangles[i]` is in the form <code>[start<sub>x</sub>, start<sub>y</sub>, end<sub>x</sub>, end<sub>y</sub>]</code>, representing a rectangle on the grid. Each rectangle is defined as follows:

*   <code>(start<sub>x</sub>, start<sub>y</sub>)</code>: The bottom-left corner of the rectangle.
*   <code>(end<sub>x</sub>, end<sub>y</sub>)</code>: The top-right corner of the rectangle.

**Note** that the rectangles do not overlap. Your task is to determine if it is possible to make **either two horizontal or two vertical cuts** on the grid such that:

*   Each of the three resulting sections formed by the cuts contains **at least** one rectangle.
*   Every rectangle belongs to **exactly** one section.

Return `true` if such cuts can be made; otherwise, return `false`.

**Example 1:**

**Input:** n = 5, rectangles = \[\[1,0,5,2],[0,2,2,4],[3,2,5,3],[0,4,4,5]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/10/23/tt1drawio.png)

The grid is shown in the diagram. We can make horizontal cuts at `y = 2` and `y = 4`. Hence, output is true.

**Example 2:**

**Input:** n = 4, rectangles = \[\[0,0,1,1],[2,0,3,4],[0,2,2,3],[3,0,4,3]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/10/23/tc2drawio.png)

We can make vertical cuts at `x = 2` and `x = 3`. Hence, output is true.

**Example 3:**

**Input:** n = 4, rectangles = \[\[0,2,2,4],[1,0,3,2],[2,2,3,4],[3,0,4,2],[3,2,4,4]]

**Output:** false

**Explanation:**

We cannot make two horizontal or two vertical cuts that satisfy the conditions. Hence, output is false.

**Constraints:**

*   <code>3 <= n <= 10<sup>9</sup></code>
*   <code>3 <= rectangles.length <= 10<sup>5</sup></code>
*   `0 <= rectangles[i][0] < rectangles[i][2] <= n`
*   `0 <= rectangles[i][1] < rectangles[i][3] <= n`
*   No two rectangles overlap.

## Solution

```kotlin
import kotlin.math.max

@Suppress("unused")
class Solution {
    fun checkValidCuts(n: Int, rectangles: Array<IntArray>): Boolean {
        val m = rectangles.size
        val xAxis = Array<IntArray>(m) { IntArray(2) }
        val yAxis = Array<IntArray>(m) { IntArray(2) }
        var ind = 0
        for (axis in rectangles) {
            val startX = axis[0]
            val startY = axis[1]
            val endX = axis[2]
            val endY = axis[3]
            xAxis[ind] = intArrayOf(startX, endX)
            yAxis[ind] = intArrayOf(startY, endY)
            ind++
        }

        xAxis.sortWith<IntArray>(
            Comparator { a: IntArray, b: IntArray -> if (a[0] == b[0]) a[1] - b[1] else a[0] - b[0] },
        )

        yAxis.sortWith<IntArray>(
            Comparator { a: IntArray, b: IntArray -> if (a[0] == b[0]) a[1] - b[1] else a[0] - b[0] },
        )
        val verticalCuts = findSections(xAxis)
        if (verticalCuts > 2) {
            return true
        }
        val horizontalCuts = findSections(yAxis)
        return horizontalCuts > 2
    }

    private fun findSections(axis: Array<IntArray>): Int {
        var end = axis[0][1]
        var sections = 1
        for (i in 1..<axis.size) {
            if (end > axis[i][0]) {
                end = max(end, axis[i][1])
            } else {
                sections++
                end = axis[i][1]
            }
            if (sections > 2) {
                return sections
            }
        }
        return sections
    }
}
```