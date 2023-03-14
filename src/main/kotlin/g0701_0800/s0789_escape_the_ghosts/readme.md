[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 789\. Escape The Ghosts

Medium

You are playing a simplified PAC-MAN game on an infinite 2-D grid. You start at the point `[0, 0]`, and you are given a destination point <code>target = [x<sub>target</sub>, y<sub>target</sub>]</code> that you are trying to get to. There are several ghosts on the map with their starting positions given as a 2D array `ghosts`, where <code>ghosts[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the starting position of the <code>i<sup>th</sup></code> ghost. All inputs are **integral coordinates**.

Each turn, you and all the ghosts may independently choose to either **move 1 unit** in any of the four cardinal directions: north, east, south, or west, or **stay still**. All actions happen **simultaneously**.

You escape if and only if you can reach the target **before** any ghost reaches you. If you reach any square (including the target) at the **same time** as a ghost, it **does not** count as an escape.

Return `true` _if it is possible to escape regardless of how the ghosts move, otherwise return_ `false`_._

**Example 1:**

**Input:** ghosts = \[\[1,0],[0,3]], target = [0,1]

**Output:** true

**Explanation:** You can reach the destination (0, 1) after 1 turn, while the ghosts located at (1, 0) and (0, 3) cannot catch up with you.

**Example 2:**

**Input:** ghosts = \[\[1,0]], target = [2,0]

**Output:** false

**Explanation:** You need to reach the destination (2, 0), but the ghost at (1, 0) lies between you and the destination.

**Example 3:**

**Input:** ghosts = \[\[2,0]], target = [1,0]

**Output:** false

**Explanation:** The ghost can reach the target at the same time as you.

**Constraints:**

*   `1 <= ghosts.length <= 100`
*   `ghosts[i].length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>
*   There can be **multiple ghosts** in the same location.
*   `target.length == 2`
*   <code>-10<sup>4</sup> <= x<sub>target</sub>, y<sub>target</sub> <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun escapeGhosts(ghosts: Array<IntArray>, target: IntArray): Boolean {
        val currPos = intArrayOf(0, 0)
        val selfDist = getDist(currPos, target)
        for (ghost in ghosts) {
            val ghostDist = getDist(ghost, target)
            if (ghostDist <= selfDist) {
                return false
            }
        }
        return true
    }

    private fun getDist(p1: IntArray, p2: IntArray): Int {
        return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    }
}
```