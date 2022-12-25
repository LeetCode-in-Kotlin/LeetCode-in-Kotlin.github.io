[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 403\. Frog Jump

Hard

A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of `stones`' positions (in units) in sorted **ascending order**, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be `1` unit.

If the frog's last jump was `k` units, its next jump must be either `k - 1`, `k`, or `k + 1` units. The frog can only jump in the forward direction.

**Example 1:**

**Input:** stones = [0,1,3,5,6,8,12,17]

**Output:** true

**Explanation:** The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.

**Example 2:**

**Input:** stones = [0,1,2,3,4,8,9,11]

**Output:** false

**Explanation:** There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.

**Constraints:**

*   `2 <= stones.length <= 2000`
*   <code>0 <= stones[i] <= 2<sup>31</sup> - 1</code>
*   `stones[0] == 0`
*   `stones` is sorted in a strictly increasing order.

## Solution

```kotlin
import java.util.HashMap
import java.util.HashSet

class Solution {
    // global hashmap to store visited index -> set of jump lengths from that index
    private val visited: HashMap<Int, HashSet<Int>> = HashMap()
    fun canCross(stones: IntArray): Boolean {
        // a mathematical check before going in the recursion
        for (i in 3 until stones.size) {
            if (stones[i] > stones[i - 1] * 2) {
                return false
            }
        }
        // map of values -> index to make sure we get the next index quickly
        val rocks: HashMap<Int, Int> = HashMap()
        for (i in stones.indices) {
            rocks.put(stones[i], i)
        }
        return jump(stones, 0, 1, 0, rocks)
    }

    private fun jump(
        stones: IntArray,
        index: Int,
        jumpLength: Int,
        expectedVal: Int,
        rocks: Map<Int, Int>
    ): Boolean {
        // overshoot and going backwards not allowed
        if (index >= stones.size || jumpLength <= 0) {
            return false
        }
        // reached the last index
        if (index == stones.size - 1) {
            return expectedVal == stones[index]
        }
        // check if this index -> jumpLength pair was seen before, otherwise record it
        val rememberJumps: HashSet<Int> = visited.getOrDefault(index, HashSet())
        if (stones[index] > expectedVal || rememberJumps.contains(jumpLength)) {
            return false
        }
        rememberJumps.add(jumpLength)
        visited.put(index, rememberJumps)
        // check for jumpLength-1, jumpLength, jumpLength+1 for a new expected value
        return (
            jump(
                stones,
                rocks[stones[index] + jumpLength] ?: stones.size,
                jumpLength + 1,
                stones[index] + jumpLength,
                rocks
            ) ||
                jump(
                    stones,
                    rocks[stones[index] + jumpLength] ?: stones.size,
                    jumpLength,
                    stones[index] + jumpLength,
                    rocks
                ) ||
                jump(
                    stones,
                    rocks[stones[index] + jumpLength] ?: stones.size,
                    jumpLength - 1,
                    stones[index] + jumpLength,
                    rocks
                )
            )
    }
}
```