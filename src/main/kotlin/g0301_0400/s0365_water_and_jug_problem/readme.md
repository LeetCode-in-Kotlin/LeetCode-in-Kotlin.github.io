[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 365\. Water and Jug Problem

Medium

You are given two jugs with capacities `jug1Capacity` and `jug2Capacity` liters. There is an infinite amount of water supply available. Determine whether it is possible to measure exactly `targetCapacity` liters using these two jugs.

If `targetCapacity` liters of water are measurable, you must have `targetCapacity` liters of water contained **within one or both buckets** by the end.

Operations allowed:

*   Fill any of the jugs with water.
*   Empty any of the jugs.
*   Pour water from one jug into another till the other jug is completely full, or the first jug itself is empty.

**Example 1:**

**Input:** jug1Capacity = 3, jug2Capacity = 5, targetCapacity = 4

**Output:** true

**Explanation:** The famous [Die Hard](https://www.youtube.com/watch?v=BVtQNK_ZUJg&ab_channel=notnek01) example

**Example 2:**

**Input:** jug1Capacity = 2, jug2Capacity = 6, targetCapacity = 5

**Output:** false

**Example 3:**

**Input:** jug1Capacity = 1, jug2Capacity = 2, targetCapacity = 3

**Output:** true

**Constraints:**

*   <code>1 <= jug1Capacity, jug2Capacity, targetCapacity <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    private fun gcd(n1: Int, n2: Int): Int {
        return if (n2 == 0) {
            n1
        } else {
            gcd(n2, n1 % n2)
        }
    }

    fun canMeasureWater(jug1Capacity: Int, jug2Capacity: Int, targetCapacity: Int): Boolean {
        if (jug1Capacity + jug2Capacity < targetCapacity) {
            return false
        }
        val gcd = gcd(jug1Capacity, jug2Capacity)
        return targetCapacity % gcd == 0
    }
}
```