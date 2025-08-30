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
            return handleSingleRobot(robots[0], distance[0], walls)
        }
        val arr = buildRobotArray(robots, distance)
        arr.sortWith { a: IntArray, b: IntArray -> if (a[0] == b[0]) (a[1] - b[1]) else (a[0] - b[0]) }
        walls.sort()
        return processMultipleRobots(arr, walls)
    }

    private fun handleSingleRobot(robot: Int, dist: Int, walls: IntArray): Int {
        var left = 0
        var right = 0
        for (wall in walls) {
            if (wall < robot - dist || wall > robot + dist) {
                continue
            }
            if (wall < robot) {
                left++
            } else if (wall > robot) {
                right++
            } else {
                left++
                right++
            }
        }
        return max(left, right)
    }

    private fun buildRobotArray(robots: IntArray, distance: IntArray): Array<IntArray> {
        val arr = Array(robots.size) { IntArray(2) }
        for (i in robots.indices) {
            arr[i][0] = robots[i]
            arr[i][1] = distance[i]
        }
        return arr
    }

    private fun processMultipleRobots(arr: Array<IntArray>, walls: IntArray): Int {
        var a: Int
        var b: Int
        var i = 0
        var j = 0
        i = skipWallsBeforeRange(walls, i, arr[j][0] - arr[j][1])
        a = countWallsUpToRobot(walls, i, arr[j][0])
        i += a
        if (i > 0 && walls[i - 1] == arr[j][0]) {
            i--
        }
        b = countWallsInRange(walls, i, arr[j][0] + arr[j][1], arr[j + 1][0])
        i += b
        j++
        while (j < arr.size) {
            val result = processRobotStep(arr, walls, j, i, a, b)
            a = result[0]
            b = result[1]
            i = result[2]
            j++
        }
        return max(a, b)
    }

    private fun skipWallsBeforeRange(walls: IntArray, i: Int, limit: Int): Int {
        var i = i
        while (i < walls.size && walls[i] < limit) {
            i++
        }
        return i
    }

    private fun countWallsUpToRobot(walls: IntArray, i: Int, robotPos: Int): Int {
        var count = 0
        while (i + count < walls.size && walls[i + count] <= robotPos) {
            count++
        }
        return count
    }

    private fun countWallsInRange(walls: IntArray, i: Int, maxReach: Int, nextRobot: Int): Int {
        var count = 0
        while (i + count < walls.size && walls[i + count] <= maxReach && walls[i + count] < nextRobot) {
            count++
        }
        return count
    }

    private fun processRobotStep(arr: Array<IntArray>, walls: IntArray, j: Int, i: Int, a: Int, b: Int): IntArray {
        var i = i
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
        val bNext = max(a, b) + r
        return intArrayOf(aNext, bNext, i)
    }
}
```