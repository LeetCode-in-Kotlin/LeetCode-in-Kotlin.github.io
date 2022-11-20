[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 335\. Self Crossing

Hard

You are given an array of integers `distance`.

You start at point `(0,0)` on an **X-Y** plane and you move `distance[0]` meters to the north, then `distance[1]` meters to the west, `distance[2]` meters to the south, `distance[3]` meters to the east, and so on. In other words, after each move, your direction changes counter-clockwise.

Return `true` if your path crosses itself, and `false` if it does not.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/selfcross1-plane.jpg)

**Input:** distance = [2,1,1,2]

**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/selfcross2-plane.jpg)

**Input:** distance = [1,2,3,4]

**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/14/selfcross3-plane.jpg)

**Input:** distance = [1,1,1,1]

**Output:** true

**Constraints:**

*   <code>1 <= distance.length <= 10<sup>5</sup></code>
*   <code>1 <= distance[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun isSelfCrossing(x: IntArray): Boolean {
        if (x.size < 4) {
            return false
        }
        for (i in 3 until x.size) {
            if (x[i - 1] <= x[i - 3] && x[i] >= x[i - 2]) {
                return true
            }
            if (i > 4 && x[i] >= x[i - 2] - x[i - 4] && x[i - 1] >= x[i - 3] - x[i - 5] && x[i - 1] <= x[i - 3] &&
                x[i - 2] >= x[i - 4]
            ) {
                return true
            }
            if (i > 3 && x[i - 1] == x[i - 3] && x[i] >= x[i - 2] - x[i - 4]) {
                return true
            }
        }
        return false
    }
}
```