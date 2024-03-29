[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1727\. Largest Submatrix With Rearrangements

Medium

You are given a binary matrix `matrix` of size `m x n`, and you are allowed to rearrange the **columns** of the `matrix` in any order.

Return _the area of the largest submatrix within_ `matrix` _where **every** element of the submatrix is_ `1` _after reordering the columns optimally._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png)

**Input:** matrix = \[\[0,0,1],[1,1,1],[1,0,1]]

**Output:** 4

**Explanation:** You can rearrange the columns as shown above. The largest submatrix of 1s, in bold, has an area of 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40852-pm.png)

**Input:** matrix = \[\[1,0,1,0,1]]

**Output:** 3

**Explanation:** You can rearrange the columns as shown above. The largest submatrix of 1s, in bold, has an area of 3.

**Example 3:**

**Input:** matrix = \[\[1,1,0],[1,0,1]]

**Output:** 2

**Explanation:** Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   `matrix[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun largestSubmatrix(matrix: Array<IntArray>): Int {
        val m: Int = matrix.size
        val n: Int = matrix[0].size
        for (i in 1 until m) {
            for (j in 0 until n) {
                if (matrix[i][j] != 0) {
                    matrix[i][j] = matrix[i - 1][j] + 1
                }
            }
        }
        var count = 0
        for (ints: IntArray in matrix) {
            ints.sort()
            for (j in 1..n) {
                count = Math.max(count, j * ints[n - j])
            }
        }
        return count
    }
}
```