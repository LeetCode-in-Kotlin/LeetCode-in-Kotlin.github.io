[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3274\. Check if Two Chessboard Squares Have the Same Color

Easy

You are given two strings, `coordinate1` and `coordinate2`, representing the coordinates of a square on an `8 x 8` chessboard.

Below is the chessboard for reference.

![](https://assets.leetcode.com/uploads/2024/07/17/screenshot-2021-02-20-at-22159-pm.png)

Return `true` if these two squares have the same color and `false` otherwise.

The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first (indicating its column), and the number second (indicating its row).

**Example 1:**

**Input:** coordinate1 = "a1", coordinate2 = "c3"

**Output:** true

**Explanation:**

Both squares are black.

**Example 2:**

**Input:** coordinate1 = "a1", coordinate2 = "h3"

**Output:** false

**Explanation:**

Square `"a1"` is black and `"h3"` is white.

**Constraints:**

*   `coordinate1.length == coordinate2.length == 2`
*   `'a' <= coordinate1[0], coordinate2[0] <= 'h'`
*   `'1' <= coordinate1[1], coordinate2[1] <= '8'`

## Solution

```kotlin
class Solution {
    fun checkTwoChessboards(coordinate1: String, coordinate2: String): Boolean {
        val s1 = (coordinate1[0].code - 'a'.code) + (coordinate1[1].code - '0'.code)
        val s2 = (coordinate2[0].code - 'a'.code) + (coordinate2[1].code - '0'.code)
        return s1 % 2 == s2 % 2
    }
}
```