[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1277\. Count Square Submatrices with All Ones

Medium

Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

**Input:** matrix = [ [0,1,1,1], [1,1,1,1], [0,1,1,1] ]

**Output:** 15

**Explanation:**

There are **10** squares of side 1.

There are **4** squares of side 2.

There is **1** square of side 3.

Total number of squares = 10 + 4 + 1 = **15**.

**Example 2:**

**Input:** matrix = [ [1,0,1], [1,1,0], [1,1,0] ]

**Output:** 7

**Explanation:**

There are **6** squares of side 1.

There is **1** square of side 2.

Total number of squares = 6 + 1 = **7**.

**Constraints:**

*   `1 <= arr.length <= 300`
*   `1 <= arr[0].length <= 300`
*   `0 <= arr[i][j] <= 1`

## Solution

```kotlin
class Solution {
    fun countSquares(matrix: Array<IntArray>): Int {
        var total = 0
        for (ints in matrix) {
            total += ints[0]
        }
        for (i in 1 until matrix[0].size) {
            total += matrix[0][i]
        }
        for (i in 1 until matrix.size) {
            for (j in 1 until matrix[0].size) {
                if (matrix[i][j] == 1) {
                    matrix[i][j] = (
                        Math.min(
                            matrix[i - 1][j - 1],
                            Math.min(matrix[i - 1][j], matrix[i][j - 1])
                        ) +
                            1
                        )
                }
                total += matrix[i][j]
            }
        }
        return total
    }
}
```