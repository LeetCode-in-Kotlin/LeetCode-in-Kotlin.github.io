[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 54\. Spiral Matrix

Medium

Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

**Input:** matrix = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,2,3,6,9,8,7,4,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

**Input:** matrix = \[\[1,2,3,4],[5,6,7,8],[9,10,11,12]]

**Output:** [1,2,3,4,8,12,11,10,9,5,6,7]

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 10`
*   `-100 <= matrix[i][j] <= 100`

## Solution

```kotlin
class Solution {
    fun spiralOrder(matrix: Array<IntArray>): List<Int> {
        val list: MutableList<Int> = ArrayList()
        var r = 0
        var c = 0
        var bigR = matrix.size - 1
        var bigC: Int = matrix[0].size - 1
        while (r <= bigR && c <= bigC) {
            for (i in c..bigC) {
                list.add(matrix[r][i])
            }
            r++
            for (i in r..bigR) {
                list.add(matrix[i][bigC])
            }
            bigC--
            run {
                var i = bigC
                while (i >= c && r <= bigR) {
                    list.add(matrix[bigR][i])
                    i--
                }
            }
            bigR--
            var i = bigR
            while (i >= r && c <= bigC) {
                list.add(matrix[i][c])
                i--
            }
            c++
        }
        return list
    }
}
```