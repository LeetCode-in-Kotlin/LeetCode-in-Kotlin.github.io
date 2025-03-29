[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3382\. Maximum Area Rectangle With Point Constraints II

Hard

There are n points on an infinite plane. You are given two integer arrays `xCoord` and `yCoord` where `(xCoord[i], yCoord[i])` represents the coordinates of the <code>i<sup>th</sup></code> point.

Your task is to find the **maximum** area of a rectangle that:

*   Can be formed using **four** of these points as its corners.
*   Does **not** contain any other point inside or on its border.
*   Has its edges **parallel** to the axes.

Return the **maximum area** that you can obtain or -1 if no such rectangle is possible.

**Example 1:**

**Input:** xCoord = [1,1,3,3], yCoord = [1,3,1,3]

**Output:** 4

**Explanation:**

**![Example 1 diagram](https://assets.leetcode.com/uploads/2024/11/02/example1.png)**

We can make a rectangle with these 4 points as corners and there is no other point that lies inside or on the border. Hence, the maximum possible area would be 4.

**Example 2:**

**Input:** xCoord = [1,1,3,3,2], yCoord = [1,3,1,3,2]

**Output:** \-1

**Explanation:**

**![Example 2 diagram](https://assets.leetcode.com/uploads/2024/11/02/example2.png)**

There is only one rectangle possible is with points `[1,1], [1,3], [3,1]` and `[3,3]` but `[2,2]` will always lie inside it. Hence, returning -1.

**Example 3:**

**Input:** xCoord = [1,1,3,3,1,3], yCoord = [1,3,1,3,2,2]

**Output:** 2

**Explanation:**

**![Example 3 diagram](https://assets.leetcode.com/uploads/2024/11/02/example3.png)**

The maximum area rectangle is formed by the points `[1,3], [1,2], [3,2], [3,3]`, which has an area of 2. Additionally, the points `[1,1], [1,2], [3,1], [3,2]` also form a valid rectangle with the same area.

**Constraints:**

*   <code>1 <= xCoord.length == yCoord.length <= 2 * 10<sup>5</sup></code>
*   <code>0 <= xCoord[i], yCoord[i] <= 8 * 10<sup>7</sup></code>
*   All the given points are **unique**.

## Solution

```kotlin
import java.util.TreeSet
import kotlin.math.max

class Solution {
    fun maxRectangleArea(xCoord: IntArray, yCoord: IntArray): Long {
        if (xCoord.size < 4) {
            return -1
        }
        val pair = xCoord.zip(yCoord) { x, y -> Pair(x, y) }.toTypedArray()
        pair.sort()
        val map = HashMap<Int, Pair>()
        val yVals = TreeSet<Int>()
        var best: Long = -1
        for (i in 0..<pair.size - 1) {
            if (yVals.isNotEmpty()) {
                val y0 = pair[i].y
                var y1 = yVals.floor(y0)
                while (y1 != null) {
                    val p1: Pair = map[y1]!!
                    if (p1.y < y0) {
                        break
                    }
                    if (y1 == y0 && pair[i + 1].x == pair[i].x && pair[i + 1].y == p1.y) {
                        val dY = p1.y - y0.toLong()
                        val dX = pair[i].x - p1.x.toLong()
                        best = max(dY * dX, best)
                    }
                    if (p1.x != pair[i].x) {
                        yVals.remove(y1)
                    }
                    y1 = yVals.lower(y1)
                }
            }
            if (pair[i].x == pair[i + 1].x) {
                yVals.add(pair[i].y)
                map.put(pair[i].y, pair[i + 1])
            }
        }
        return best
    }

    private class Pair(val x: Int, val y: Int) : Comparable<Pair> {
        override fun compareTo(other: Pair): Int {
            return if (x == other.x) y - other.y else x - other.x
        }
    }
}
```