[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2139\. Minimum Moves to Reach Target Score

Medium

You are playing a game with integers. You start with the integer `1` and you want to reach the integer `target`.

In one move, you can either:

*   **Increment** the current integer by one (i.e., `x = x + 1`).
*   **Double** the current integer (i.e., `x = 2 * x`).

You can use the **increment** operation **any** number of times, however, you can only use the **double** operation **at most** `maxDoubles` times.

Given the two integers `target` and `maxDoubles`, return _the minimum number of moves needed to reach_ `target` _starting with_ `1`.

**Example 1:**

**Input:** target = 5, maxDoubles = 0

**Output:** 4

**Explanation:** Keep incrementing by 1 until you reach target.

**Example 2:**

**Input:** target = 19, maxDoubles = 2

**Output:** 7

**Explanation:** Initially, x = 1 

Increment 3 times so x = 4 

Double once so x = 8 

Increment once so x = 9 

Double again so x = 18 

Increment once so x = 19

**Example 3:**

**Input:** target = 10, maxDoubles = 4

**Output:** 4

**Explanation:** Initially, x = 1 

Increment once so x = 2 

Double once so x = 4 

Increment once so x = 5 

Double again so x = 10

**Constraints:**

*   <code>1 <= target <= 10<sup>9</sup></code>
*   `0 <= maxDoubles <= 100`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun minMoves(target: Int, maxDoubles: Int): Int {
        var target = target
        var maxDoubles = maxDoubles
        var count = 0
        while (target > 1) {
            if (maxDoubles > 0 && target % 2 == 0) {
                maxDoubles--
                target /= 2
            } else {
                if (maxDoubles == 0) {
                    count = count + target - 1
                    return count
                } else {
                    target -= 1
                }
            }
            count++
        }
        return count
    }
}
```