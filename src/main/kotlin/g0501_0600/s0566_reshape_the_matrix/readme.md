[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 566\. Reshape the Matrix

Easy

In MATLAB, there is a handy function called `reshape` which can reshape an `m x n` matrix into a new one with a different size `r x c` keeping its original data.

You are given an `m x n` matrix `mat` and two integers `r` and `c` representing the number of rows and the number of columns of the wanted reshaped matrix.

The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were.

If the `reshape` operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg)

**Input:** mat = \[\[1,2],[3,4]], r = 1, c = 4

**Output:** [[1,2,3,4]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg)

**Input:** mat = \[\[1,2],[3,4]], r = 2, c = 4

**Output:** [[1,2],[3,4]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 100`
*   `-1000 <= mat[i][j] <= 1000`
*   `1 <= r, c <= 300`

## Solution

```kotlin
class Solution {
    fun matrixReshape(mat: Array<IntArray>, r: Int, c: Int): Array<IntArray> {
        if (mat.size * mat[0].size != r * c) {
            return mat
        }
        var p = 0
        val flatArr = IntArray(mat.size * mat[0].size)
        for (ints in mat) {
            for (anInt in ints) {
                flatArr[p++] = anInt
            }
        }
        val ansMat = Array(r) { IntArray(c) }
        var k = 0
        for (i in ansMat.indices) {
            for (j in ansMat[i].indices) {
                ansMat[i][j] = flatArr[k++]
            }
        }
        return ansMat
    }
}
```