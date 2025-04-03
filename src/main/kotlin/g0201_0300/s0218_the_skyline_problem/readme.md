[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 218\. The Skyline Problem

Hard

A city's **skyline** is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return _the **skyline** formed by these buildings collectively_.

The geometric information of each building is given in the array `buildings` where <code>buildings[i] = [left<sub>i</sub>, right<sub>i</sub>, height<sub>i</sub>]</code>:

*   <code>left<sub>i</sub></code> is the x coordinate of the left edge of the <code>i<sup>th</sup></code> building.
*   <code>right<sub>i</sub></code> is the x coordinate of the right edge of the <code>i<sup>th</sup></code> building.
*   <code>height<sub>i</sub></code> is the height of the <code>i<sup>th</sup></code> building.

You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.

The **skyline** should be represented as a list of "key points" **sorted by their x-coordinate** in the form <code>[[x<sub>1</sub>,y<sub>1</sub>],[x<sub>2</sub>,y<sub>2</sub>],...]</code>. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate `0` and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

**Note:** There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...,[2 3],[4 5],[7 5],[11 5],[12 7],...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...,[2 3],[4 5],[12 7],...]`

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)

**Input:** buildings = \[\[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]

**Output:** [[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]

**Explanation:** Figure A shows the buildings of the input. Figure B shows the skyline formed by those buildings. The red points in figure B represent the key points in the output list.

**Example 2:**

**Input:** buildings = \[\[0,2,3],[2,5,3]]

**Output:** [[0,3],[5,0]]

**Constraints:**

*   <code>1 <= buildings.length <= 10<sup>4</sup></code>
*   <code>0 <= left<sub>i</sub> < right<sub>i</sub> <= 2<sup>31</sup> - 1</code>
*   <code>1 <= height<sub>i</sub> <= 2<sup>31</sup> - 1</code>
*   `buildings` is sorted by <code>left<sub>i</sub></code> in non-decreasing order.

## Solution

```kotlin
import java.util.TreeMap

class Solution {
    fun getSkyline(blds: Array<IntArray>): List<List<Int>> {
        val ans = ArrayList<List<Int>>()
        if (blds.isEmpty()) {
            return ans
        }
        val totalBuildings = blds.size
        val buildings = Array(totalBuildings * 2) { i ->
            if (i % 2 == 0) {
                Building(blds[i / 2][0], blds[i / 2][2], true)
            } else {
                Building(blds[i / 2][1], blds[i / 2][2], false)
            }
        }
        buildings.sort()
        val skyline = TreeMap<Int, Int>()
        skyline[0] = 1
        var prevMaxHeight = 0
        for (building in buildings) {
            val height = building.height
            if (building.isStart) {
                skyline[height] = 1 + (skyline[height] ?: 0)
            } else {
                skyline[height] = (skyline[height] ?: 1) - 1
                if (skyline[height] == 0) skyline.remove(height)
            }
            val xCoor = building.xCoor
            val curMaxHeight = skyline.lastKey()
            if (prevMaxHeight != curMaxHeight) {
                ans.add(arrayListOf(xCoor, curMaxHeight))
                prevMaxHeight = curMaxHeight
            }
        }
        return ans
    }

    private class Building(val xCoor: Int, val height: Int, val isStart: Boolean) : Comparable<Building> {
        override fun compareTo(other: Building): Int {
            return COMPARATOR.compare(this, other)
        }

        private companion object {
            private val COMPARATOR = Comparator<Building> { a, b ->
                when {
                    a.xCoor != b.xCoor -> a.xCoor.compareTo(b.xCoor)
                    a.isStart && b.isStart -> b.height.compareTo(a.height)
                    !(a.isStart || b.isStart) -> a.height.compareTo(b.height)
                    else -> if (a.isStart) -1 else 1
                }
            }
        }
    }
}
```