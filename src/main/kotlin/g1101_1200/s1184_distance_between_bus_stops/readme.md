[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1184\. Distance Between Bus Stops

Easy

A bus has `n` stops numbered from `0` to `n - 1` that form a circle. We know the distance between all pairs of neighboring stops where `distance[i]` is the distance between the stops number `i` and `(i + 1) % n`.

The bus goes along both directions i.e. clockwise and counterclockwise.

Return the shortest distance between the given `start` and `destination` stops.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/09/03/untitled-diagram-1.jpg)

**Input:** distance = [1,2,3,4], start = 0, destination = 1

**Output:** 1

**Explanation:** Distance between 0 and 1 is 1 or 9, minimum is 1.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/09/03/untitled-diagram-1-1.jpg)

**Input:** distance = [1,2,3,4], start = 0, destination = 2

**Output:** 3

**Explanation:** Distance between 0 and 2 is 3 or 7, minimum is 3.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/09/03/untitled-diagram-1-2.jpg)

**Input:** distance = [1,2,3,4], start = 0, destination = 3

**Output:** 4

**Explanation:** Distance between 0 and 3 is 6 or 4, minimum is 4.

**Constraints:**

*   `1 <= n <= 10^4`
*   `distance.length == n`
*   `0 <= start, destination < n`
*   `0 <= distance[i] <= 10^4`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun distanceBetweenBusStops(distance: IntArray, start: Int, destination: Int): Int {
        var start = start
        var destination = destination
        if (start > destination) {
            val tmp = start
            start = destination
            destination = tmp
        }
        var clockwise = 0
        for (i in start until destination) {
            clockwise += distance[i]
        }
        var counterClockwise = 0
        for (i in destination until distance.size) {
            counterClockwise += distance[i]
        }
        for (i in 0 until start) {
            counterClockwise += distance[i]
        }
        return Math.min(clockwise, counterClockwise)
    }
}
```