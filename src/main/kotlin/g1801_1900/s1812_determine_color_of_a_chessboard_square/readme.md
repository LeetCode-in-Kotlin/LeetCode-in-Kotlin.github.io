[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1812\. Determine Color of a Chessboard Square

Easy

You are given `coordinates`, a string that represents the coordinates of a square of the chessboard. Below is a chessboard for your reference.

![](https://assets.leetcode.com/uploads/2021/02/19/screenshot-2021-02-20-at-22159-pm.png)

Return `true` _if the square is white, and_ `false` _if the square is black_.

The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first, and the number second.

**Example 1:**

**Input:** coordinates = "a1"

**Output:** false

**Explanation:** From the chessboard above, the square with coordinates "a1" is black, so return false.

**Example 2:**

**Input:** coordinates = "h3"

**Output:** true

**Explanation:** From the chessboard above, the square with coordinates "h3" is white, so return true.

**Example 3:**

**Input:** coordinates = "c7"

**Output:** false

**Constraints:**

*   `coordinates.length == 2`
*   `'a' <= coordinates[0] <= 'h'`
*   `'1' <= coordinates[1] <= '8'`

## Solution

```kotlin
class Solution {
    fun squareIsWhite(coordinates: String): Boolean {
        val x = coordinates[0]
        val y = (coordinates[1].toString() + "").toInt()
        return when (x) {
            'a', 'c', 'e', 'g' -> y % 2 == 0
            else -> y % 2 != 0
        }
    }
}
```