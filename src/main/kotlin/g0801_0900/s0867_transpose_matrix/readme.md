[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 867\. Transpose Matrix

Easy

Given a 2D integer array `matrix`, return _the **transpose** of_ `matrix`.

The **transpose** of a matrix is the matrix flipped over its main diagonal, switching the matrix's row and column indices.

![](https://assets.leetcode.com/uploads/2021/02/10/hint_transpose.png)

**Example 1:**

**Input:** matrix = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [[1,4,7],[2,5,8],[3,6,9]]

**Example 2:**

**Input:** matrix = \[\[1,2,3],[4,5,6]]

**Output:** [[1,4],[2,5],[3,6]]

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 1000`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= matrix[i][j] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun transpose(input: Array<IntArray>): Array<IntArray> {
        val output = Array(input[0].size) {
            IntArray(
                input.size,
            )
        }
        var i = 0
        var b = 0
        while (i < input.size) {
            var j = 0
            var a = 0
            while (j < input[0].size) {
                output[a][b] = input[i][j]
                j++
                a++
            }
            i++
            b++
        }
        return output
    }
}
```