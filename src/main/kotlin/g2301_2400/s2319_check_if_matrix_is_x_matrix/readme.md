[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2319\. Check if Matrix Is X-Matrix

Easy

A square matrix is said to be an **X-Matrix** if **both** of the following conditions hold:

1.  All the elements in the diagonals of the matrix are **non-zero**.
2.  All other elements are 0.

Given a 2D integer array `grid` of size `n x n` representing a square matrix, return `true` _if_ `grid` _is an X-Matrix_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/05/03/ex1.jpg)

**Input:** grid = \[\[2,0,0,1],[0,3,1,0],[0,5,2,0],[4,0,0,2]]

**Output:** true

**Explanation:** Refer to the diagram above.

An X-Matrix should have the green elements (diagonals) be non-zero and the red elements be 0.

Thus, grid is an X-Matrix.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/05/03/ex2.jpg)

**Input:** grid = \[\[5,7,0],[0,3,1],[0,5,0]]

**Output:** false

**Explanation:** Refer to the diagram above.

An X-Matrix should have the green elements (diagonals) be non-zero and the red elements be 0.

Thus, grid is not an X-Matrix.

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `3 <= n <= 100`
*   <code>0 <= grid[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun checkXMatrix(grid: Array<IntArray>): Boolean {
        for (i in grid.indices) {
            for (j in grid[0].indices) {
                if (i == j || i + j == grid.size - 1) {
                    if (grid[i][j] == 0) {
                        return false
                    }
                } else {
                    if (grid[i][j] != 0) {
                        return false
                    }
                }
            }
        }
        return true
    }
}
```