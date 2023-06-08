[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1380\. Lucky Numbers in a Matrix

Easy

Given an `m x n` matrix of **distinct** numbers, return _all **lucky numbers** in the matrix in **any** order_.

A **lucky number** is an element of the matrix such that it is the minimum element in its row and maximum in its column.

**Example 1:**

**Input:** matrix = \[\[3,7,8],[9,11,13],[15,16,17]]

**Output:** [15]

**Explanation:** 15 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Example 2:**

**Input:** matrix = \[\[1,10,4,2],[9,3,8,7],[15,16,17,12]]

**Output:** [12]

**Explanation:** 12 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Example 3:**

**Input:** matrix = \[\[7,8],[1,2]]

**Output:** [7]

**Explanation:** 7 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= n, m <= 50`
*   <code>1 <= matrix[i][j] <= 10<sup>5</sup></code>.
*   All elements in the matrix are distinct.

## Solution

```kotlin
class Solution {
    fun luckyNumbers(matrix: Array<IntArray>): List<Int> {
        val mini: MutableList<Int> = ArrayList()
        val maxi: MutableList<Int> = ArrayList()
        for (arr in matrix) {
            var min = Int.MAX_VALUE
            for (j in arr) {
                if (min > j) {
                    min = j
                }
            }
            mini.add(min)
        }
        val cols = matrix[0].size
        for (c in 0 until cols) {
            var max = Int.MIN_VALUE
            for (ints in matrix) {
                if (ints[c] > max) {
                    max = ints[c]
                }
            }
            maxi.add(max)
        }
        val res: MutableList<Int> = ArrayList()
        for (value in mini) {
            if (maxi.contains(value)) {
                res.add(value)
            }
        }
        return res
    }
}
```