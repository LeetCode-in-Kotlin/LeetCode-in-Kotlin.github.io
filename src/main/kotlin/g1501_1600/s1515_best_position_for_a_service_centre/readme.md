[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1515\. Best Position for a Service Centre

Hard

A delivery company wants to build a new service center in a new city. The company knows the positions of all the customers in this city on a 2D-Map and wants to build the new center in a position such that **the sum of the euclidean distances to all customers is minimum**.

Given an array `positions` where <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> is the position of the `ith` customer on the map, return _the minimum sum of the euclidean distances_ to all customers.

In other words, you need to choose the position of the service center <code>[x<sub>centre</sub>, y<sub>centre</sub>]</code> such that the following formula is minimized:

![](https://assets.leetcode.com/uploads/2020/06/25/q4_edited.jpg)

Answers within <code>10<sup>-5</sup></code> of the actual value will be accepted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/25/q4_e1.jpg)

**Input:** positions = \[\[0,1],[1,0],[1,2],[2,1]]

**Output:** 4.00000

**Explanation:** As shown, you can see that choosing [x<sub>centre</sub>, y<sub>centre</sub>] = [1, 1] will make the distance to each customer = 1, the sum of all distances is 4 which is the minimum possible we can achieve.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/06/25/q4_e3.jpg)

**Input:** positions = \[\[1,1],[3,3]]

**Output:** 2.82843

**Explanation:** The minimum possible sum of distances = sqrt(2) + sqrt(2) = 2.82843

**Constraints:**

*   `1 <= positions.length <= 50`
*   `positions[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>

## Solution

```kotlin
class Solution {
    fun getMinDistSum(positions: Array<IntArray>): Double {
        var minX = Int.MAX_VALUE.toDouble()
        var minY = Int.MAX_VALUE.toDouble()
        var maxX = Int.MIN_VALUE.toDouble()
        var maxY = Int.MIN_VALUE.toDouble()
        for (pos in positions) {
            maxX = Math.max(maxX, pos[0].toDouble())
            maxY = Math.max(maxY, pos[1].toDouble())
            minX = Math.min(minX, pos[0].toDouble())
            minY = Math.min(minY, pos[1].toDouble())
        }
        var xMid = minX + (maxX - minX) / 2
        var yMid = minY + (maxY - minY) / 2
        var jump = Math.max(maxX - minX, maxY - minY)
        var ans = getTotalDistance(xMid, yMid, positions)
        while (jump > 0.00001) {
            val list = getFourCorners(xMid, yMid, jump)
            var found = false
            for (point in list) {
                val pointAns = getTotalDistance(point[0], point[1], positions)
                if (ans > pointAns) {
                    xMid = point[0]
                    yMid = point[1]
                    ans = pointAns
                    found = true
                }
            }
            if (!found) {
                jump = jump / 2
            }
        }
        return ans
    }

    private fun getFourCorners(xMid: Double, yMid: Double, jump: Double): List<DoubleArray> {
        val list: MutableList<DoubleArray> = ArrayList()
        list.add(doubleArrayOf(xMid - jump, yMid + jump))
        list.add(doubleArrayOf(xMid + jump, yMid + jump))
        list.add(doubleArrayOf(xMid - jump, yMid - jump))
        list.add(doubleArrayOf(xMid + jump, yMid - jump))
        return list
    }

    private fun getTotalDistance(x: Double, y: Double, positions: Array<IntArray>): Double {
        var totalDistanceSum = 0.0
        for (point in positions) {
            val xDistance = x - point[0]
            val yDistance = y - point[1]
            totalDistanceSum += Math.sqrt(xDistance * xDistance + yDistance * yDistance)
        }
        return totalDistanceSum
    }
}
```