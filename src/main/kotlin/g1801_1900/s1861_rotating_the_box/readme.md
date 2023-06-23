[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1861\. Rotating the Box

Medium

You are given an `m x n` matrix of characters `box` representing a side-view of a box. Each cell of the box is one of the following:

*   A stone `'#'`
*   A stationary obstacle `'*'`
*   Empty `'.'`

The box is rotated **90 degrees clockwise**, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity **does not** affect the obstacles' positions, and the inertia from the box's rotation **does not** affect the stones' horizontal positions.

It is **guaranteed** that each stone in `box` rests on an obstacle, another stone, or the bottom of the box.

Return _an_ `n x m` _matrix representing the box after the rotation described above_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

**Input:**

    box = \[\["#",".","#"]]

**Output:**

    [["."], 
     ["#"], 
     ["#"]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

**Input:**

    box = \[\["#",".","*","."], 
           ["#","#","*","."]]

**Output:**

    [["#","."], 
     ["#","#"], 
     ["*","*"], 
     [".","."]]

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

**Input:**

    box = \[\["#","#","*",".","*","."], 
           ["#","#","#","*",".","."], 
           ["#","#","#",".","#","."]]

**Output:**

    [[".","#","#"], 
     [".","#","#"], 
     ["#","#","*"], 
     ["#","*","."], 
     ["#",".","*"], 
     ["#",".","."]]

**Constraints:**

*   `m == box.length`
*   `n == box[i].length`
*   `1 <= m, n <= 500`
*   `box[i][j]` is either `'#'`, `'*'`, or `'.'`.

## Solution

```kotlin
class Solution {
    fun rotateTheBox(box: Array<CharArray>): Array<CharArray> {
        val n = box.size
        val m = box[0].size
        val result = Array(m) { CharArray(n) }
        for (i in 0 until n) {
            var j = m - 1
            var idx = m - 1
            while (j >= 0) {
                if (box[i][j] == '#') {
                    result[j--][n - i - 1] = '.'
                    result[idx--][n - i - 1] = '#'
                } else {
                    val c = box[i][j]
                    result[j--][n - i - 1] = c
                    if (c == '*') {
                        idx = j
                    }
                }
            }
        }
        return result
    }
}
```