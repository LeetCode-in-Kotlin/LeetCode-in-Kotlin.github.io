[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3661\. Maximum Walls Destroyed by Robots

Hard

There is an endless straight line populated with some robots and walls. You are given integer arrays `robots`, `distance`, and `walls`:

*   `robots[i]` is the position of the <code>i<sup>th</sup></code> robot.
*   `distance[i]` is the **maximum** distance the <code>i<sup>th</sup></code> robot's bullet can travel.
*   `walls[j]` is the position of the <code>j<sup>th</sup></code> wall.

Every robot has **one** bullet that can either fire to the left or the right **at most** `distance[i]` meters.

A bullet destroys every wall in its path that lies within its range. Robots are fixed obstacles: if a bullet hits another robot before reaching a wall, it **immediately stops** at that robot and cannot continue.

Return the **maximum** number of **unique** walls that can be destroyed by the robots.

Notes:

*   A wall and a robot may share the same position; the wall can be destroyed by the robot at that position.
*   Robots are not destroyed by bullets.

**Example 1:**

**Input:** robots = [4], distance = [3], walls = [1,10]

**Output:** 1

**Explanation:**

*   `robots[0] = 4` fires **left** with `distance[0] = 3`, covering `[1, 4]` and destroys `walls[0] = 1`.
*   Thus, the answer is 1.

**Example 2:**

**Input:** robots = [10,2], distance = [5,1], walls = [5,2,7]

**Output:** 3

**Explanation:**

*   `robots[0] = 10` fires **left** with `distance[0] = 5`, covering `[5, 10]` and destroys `walls[0] = 5` and `walls[2] = 7`.
*   `robots[1] = 2` fires **left** with `distance[1] = 1`, covering `[1, 2]` and destroys `walls[1] = 2`.
*   Thus, the answer is 3.

**Example 3:**

**Input:** robots = [1,2], distance = [100,1], walls = [10]

**Output:** 0

**Explanation:**

In this example, only `robots[0]` can reach the wall, but its shot to the **right** is blocked by `robots[1]`; thus the answer is 0.

**Constraints:**

*   <code>1 <= robots.length == distance.length <= 10<sup>5</sup></code>
*   <code>1 <= walls.length <= 10<sup>5</sup></code>
*   <code>1 <= robots[i], walls[j] <= 10<sup>9</sup></code>
*   <code>1 <= distance[i] <= 10<sup>5</sup></code>
*   All values in `robots` are **unique**
*   All values in `walls` are **unique**

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxWalls(robots: IntArray, distance: IntArray, walls: IntArray): Int {
        if (robots.size == 1) {
            var a = 0
            var b = 0
            for (wall in walls) {
                if (wall < robots[0] - distance[0] || wall > robots[0] + distance[0]) {
                    continue
                }
                if (wall < robots[0]) {
                    a++
                } else if (wall > robots[0]) {
                    b++
                } else {
                    a++
                    b++
                }
            }
            return max(a, b)
        }
        val arr = Array<IntArray>(robots.size) { i -> intArrayOf(robots[i], distance[i]) }
        arr.sortWith { a: IntArray, b: IntArray -> a[0] - b[0] }
        walls.sort()
        var a = 0
        var b = 0
        var i = 0
        var j = 0
        while (i < walls.size && walls[i] < arr[j][0] - arr[j][1]) {
            i++
        }
        while (i < walls.size && walls[i] <= arr[j][0]) {
            a++
            i++
        }
        if (i > 0 && walls[i - 1] == arr[j][0]) {
            i--
        }
        while (i < walls.size && walls[i] <= arr[j][0] + arr[j][1] && walls[i] < arr[j + 1][0]) {
            b++
            i++
        }
        j++
        while (j < arr.size) {
            var l1 = 0
            var k = i
            while (k < walls.size && walls[k] < arr[j][0] - arr[j][1]) {
                k++
            }
            while (k < walls.size && walls[k] <= arr[j][0]) {
                l1++
                k++
            }
            val nextI = k
            var l2 = l1
            k = i - 1
            while (k >= 0 && walls[k] > arr[j - 1][0] && walls[k] >= arr[j][0] - arr[j][1]) {
                l2++
                k--
            }
            val aNext = max(a + l2, b + l1)
            var r = 0
            val lim =
                if (j < arr.size - 1) {
                    min(arr[j + 1][0], arr[j][0] + arr[j][1] + 1)
                } else {
                    arr[j][0] + arr[j][1] + 1
                }
            i = if (nextI > 0 && walls[nextI - 1] == arr[j][0]) {
                nextI - 1
            } else {
                nextI
            }
            while (i < walls.size && walls[i] < lim) {
                r++
                i++
            }
            j++
            val bNext = max(a, b) + r
            a = aNext
            b = bNext
        }
        return max(a, b)
    }
}
```