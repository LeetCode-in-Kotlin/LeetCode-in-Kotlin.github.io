[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3143\. Maximum Points Inside the Square

Medium

You are given a 2D array `points` and a string `s` where, `points[i]` represents the coordinates of point `i`, and `s[i]` represents the **tag** of point `i`.

A **valid** square is a square centered at the origin `(0, 0)`, has edges parallel to the axes, and **does not** contain two points with the same tag.

Return the **maximum** number of points contained in a **valid** square.

Note:

*   A point is considered to be inside the square if it lies on or within the square's boundaries.
*   The side length of the square can be zero.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/03/29/3708-tc1.png)

**Input:** points = \[\[2,2],[-1,-2],[-4,4],[-3,1],[3,-3]], s = "abdca"

**Output:** 2

**Explanation:**

The square of side length 4 covers two points `points[0]` and `points[1]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/03/29/3708-tc2.png)

**Input:** points = \[\[1,1],[-2,-2],[-2,2]], s = "abb"

**Output:** 1

**Explanation:**

The square of side length 2 covers one point, which is `points[0]`.

**Example 3:**

**Input:** points = \[\[1,1],[-1,-1],[2,-2]], s = "ccd"

**Output:** 0

**Explanation:**

It's impossible to make any valid squares centered at the origin such that it covers only one point among `points[0]` and `points[1]`.

**Constraints:**

*   <code>1 <= s.length, points.length <= 10<sup>5</sup></code>
*   `points[i].length == 2`
*   <code>-10<sup>9</sup> <= points[i][0], points[i][1] <= 10<sup>9</sup></code>
*   `s.length == points.length`
*   `points` consists of distinct coordinates.
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxPointsInsideSquare(points: Array<IntArray>, s: String): Int {
        val tags = IntArray(26)
        tags.fill(Int.MAX_VALUE)
        var secondMin = Int.MAX_VALUE
        for (i in s.indices) {
            val dist = max(abs(points[i][0]), abs(points[i][1]))
            val c = s[i]
            if (tags[c.code - 'a'.code] == Int.MAX_VALUE) {
                tags[c.code - 'a'.code] = dist
            } else if (dist < tags[c.code - 'a'.code]) {
                secondMin = min(secondMin, tags[c.code - 'a'.code])
                tags[c.code - 'a'.code] = dist
            } else {
                secondMin = min(secondMin, dist)
            }
        }
        var count = 0
        for (dist in tags) {
            if (dist < secondMin) {
                count++
            }
        }
        return count
    }
}
```