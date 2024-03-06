[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3033\. Modify the Matrix

Easy

Given a **0-indexed** `m x n` integer matrix `matrix`, create a new **0-indexed** matrix called `answer`. Make `answer` equal to `matrix`, then replace each element with the value `-1` with the **maximum** element in its respective column.

Return _the matrix_ `answer`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/12/24/matrix1.png)

**Input:** matrix = \[\[1,2,-1],[4,-1,6],[7,8,9]]

**Output:** [[1,2,9],[4,8,6],[7,8,9]]

**Explanation:** The diagram above shows the elements that are changed (in blue). 
- We replace the value in the cell [1][1] with the maximum value in the column 1, that is 8. 
- We replace the value in the cell [0][2] with the maximum value in the column 2, that is 9.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/12/24/matrix2.png)

**Input:** matrix = \[\[3,-1],[5,2]]

**Output:** [[3,2],[5,2]]

**Explanation:** The diagram above shows the elements that are changed (in blue).

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `2 <= m, n <= 50`
*   `-1 <= matrix[i][j] <= 100`
*   The input is generated such that each column contains at least one non-negative integer.

## Solution

```kotlin
class Solution {
    fun modifiedMatrix(matrix: Array<IntArray>): Array<IntArray> {
        for (i in matrix.indices) {
            for (j in matrix[0].indices) {
                if (matrix[i][j] == -1) {
                    var y = 0
                    for (ints in matrix) {
                        if (ints[j] > y) {
                            y = ints[j]
                        }
                    }
                    matrix[i][j] = y
                }
            }
        }
        return matrix
    }
}
```