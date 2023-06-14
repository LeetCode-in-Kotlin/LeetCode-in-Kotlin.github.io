[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1232\. Check If It Is a Straight Line

Easy

You are given an array `coordinates`, `coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/15/untitled-diagram-2.jpg)

**Input:** coordinates = \[\[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]

**Output:** true

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/10/09/untitled-diagram-1.jpg)**

**Input:** coordinates = \[\[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]

**Output:** false

**Constraints:**

*   `2 <= coordinates.length <= 1000`
*   `coordinates[i].length == 2`
*   `-10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4`
*   `coordinates` contains no duplicate point.

## Solution

```kotlin
class Solution {
    fun checkStraightLine(coordinates: Array<IntArray>): Boolean {
        val deltaX1 = coordinates[0][0] - coordinates[1][0]
        val deltaY1 = coordinates[0][1] - coordinates[1][1]
        val prev = coordinates[1]
        for (i in 2 until coordinates.size) {
            val point = coordinates[i]
            val deltaX2 = point[0] - prev[0]
            val deltaY2 = point[1] - prev[1]
            if (deltaX1 * deltaY2 != deltaX2 * deltaY1) {
                return false
            }
        }
        return true
    }
}
```