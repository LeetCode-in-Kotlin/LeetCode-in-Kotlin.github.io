[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1329\. Sort the Matrix Diagonally

Medium

A **matrix diagonal** is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the **matrix diagonal** starting from `mat[2][0]`, where `mat` is a `6 x 3` matrix, includes cells `mat[2][0]`, `mat[3][1]`, and `mat[4][2]`.

Given an `m x n` matrix `mat` of integers, sort each **matrix diagonal** in ascending order and return _the resulting matrix_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

**Input:** mat = \[\[3,3,1,1],[2,2,1,2],[1,1,1,2]]

**Output:** [[1,1,1,1],[1,2,2,2],[1,2,3,3]]

**Example 2:**

**Input:** mat = \[\[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]

**Output:** [[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 100`
*   `1 <= mat[i][j] <= 100`

## Solution

```kotlin
class Solution {
    fun diagonalSort(mat: Array<IntArray>): Array<IntArray> {
        val m = mat.size
        val n = mat[0].size
        val sorted = Array(m) { IntArray(n) }
        for (i in m - 1 downTo 0) {
            var iCopy = i
            val list: MutableList<Int> = ArrayList()
            run {
                var j = 0
                while (j < n && iCopy < m) {
                    list.add(mat[iCopy][j])
                    j++
                    iCopy++
                }
            }
            list.sort()
            iCopy = i
            var j = 0
            while (j < n && iCopy < m) {
                sorted[iCopy][j] = list[j]
                j++
                iCopy++
            }
        }
        for (j in n - 1 downTo 1) {
            var jCopy = j
            val list: MutableList<Int> = ArrayList()
            run {
                var i = 0
                while (i < m && jCopy < n) {
                    list.add(mat[i][jCopy])
                    i++
                    jCopy++
                }
            }
            list.sort()
            jCopy = j
            var i = 0
            while (i < m && jCopy < n) {
                sorted[i][jCopy] = list[i]
                i++
                jCopy++
            }
        }
        return sorted
    }
}
```