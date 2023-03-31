[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 849\. Maximize Distance to Closest Person

Medium

You are given an array representing a row of `seats` where `seats[i] = 1` represents a person sitting in the <code>i<sup>th</sup></code> seat, and `seats[i] = 0` represents that the <code>i<sup>th</sup></code> seat is empty **(0-indexed)**.

There is at least one empty seat, and at least one person sitting.

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized.

Return _that maximum distance to the closest person_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/10/distance.jpg)

**Input:** seats = [1,0,0,0,1,0,1]

**Output:** 2

**Explanation:**

If Alex sits in the second open seat (i.e. seats[2]), then the closest person has distance 2.

If Alex sits in any other open seat, the closest person has distance 1.

Thus, the maximum distance to the closest person is 2.

**Example 2:**

**Input:** seats = [1,0,0,0]

**Output:** 3

**Explanation:**

If Alex sits in the last seat (i.e. seats[3]), the closest person is 3 seats away.

This is the maximum distance possible, so the answer is 3.

**Example 3:**

**Input:** seats = [0,1]

**Output:** 1

**Constraints:**

*   <code>2 <= seats.length <= 2 * 10<sup>4</sup></code>
*   `seats[i]` is `0` or `1`.
*   At least one seat is **empty**.
*   At least one seat is **occupied**.

## Solution

```kotlin
class Solution {
    private var maxDist = 0

    fun maxDistToClosest(seats: IntArray): Int {
        for (i in seats.indices) {
            if (seats[i] == 0) {
                extend(seats, i)
            }
        }
        return maxDist
    }

    private fun extend(seats: IntArray, position: Int) {
        var left = position - 1
        var right = position + 1
        var leftMinDistance = 1
        while (left >= 0) {
            if (seats[left] == 0) {
                leftMinDistance++
                left--
            } else {
                break
            }
        }
        var rightMinDistance = 1
        while (right < seats.size) {
            if (seats[right] == 0) {
                rightMinDistance++
                right++
            } else {
                break
            }
        }
        val maxReach: Int = when (position) {
            0 -> rightMinDistance
            seats.size - 1 -> leftMinDistance
            else -> leftMinDistance.coerceAtMost(rightMinDistance)
        }
        maxDist = maxDist.coerceAtLeast(maxReach)
    }
}
```