[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 304\. Range Sum Query 2D - Immutable

Medium

Given a 2D matrix `matrix`, handle multiple queries of the following type:

*   Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the NumMatrix class:

*   `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
*   `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg)

**Input**

    ["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
    [[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]

**Output:** [null, 8, 11, 12]

**Explanation:**

    NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
    numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
    numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
    numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle) 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 200`
*   <code>-10<sup>5</sup> <= matrix[i][j] <= 10<sup>5</sup></code>
*   `0 <= row1 <= row2 < m`
*   `0 <= col1 <= col2 < n`
*   At most <code>10<sup>4</sup></code> calls will be made to `sumRegion`.

## Solution

```kotlin
class NumMatrix(matrix: Array<IntArray>) {
    private val M = matrix.size
    private val N = if (M > 0) matrix[0].size else 0

    var array = Array<IntArray> (M + 1) { IntArray(N + 1) }

    init {
        for (i in 1..M) {
            for (j in 1..N) {
                array[i][j] = matrix[i - 1][j - 1] + array[i][j - 1] + array[i - 1][j] - array[i - 1][j - 1]
            }
        }
    }

    fun sumRegion(row1: Int, col1: Int, row2: Int, col2: Int): Int {
        return array[row2 + 1][col2 + 1] - array[row2 + 1][col1] - array[row1][col2 + 1] + array[row1][col1]
    }
}

/*
 * Your NumMatrix object will be instantiated and called as such:
 * var obj = NumMatrix(matrix)
 * var param_1 = obj.sumRegion(row1,col1,row2,col2)
 */
```