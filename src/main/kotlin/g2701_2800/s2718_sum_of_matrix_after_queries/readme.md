[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2718\. Sum of Matrix After Queries

Medium

You are given an integer `n` and a **0-indexed** **2D array** `queries` where <code>queries[i] = [type<sub>i</sub>, index<sub>i</sub>, val<sub>i</sub>]</code>.

Initially, there is a **0-indexed** `n x n` matrix filled with `0`'s. For each query, you must apply one of the following changes:

*   if <code>type<sub>i</sub> == 0</code>, set the values in the row with <code>index<sub>i</sub></code> to <code>val<sub>i</sub></code>, overwriting any previous values.
*   if <code>type<sub>i</sub> == 1</code>, set the values in the column with <code>index<sub>i</sub></code> to <code>val<sub>i</sub></code>, overwriting any previous values.

Return _the sum of integers in the matrix after all queries are applied_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/05/11/exm1.png)

**Input:** n = 3, queries = \[\[0,0,1],[1,2,2],[0,2,3],[1,0,4]]

**Output:** 23

**Explanation:** The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 23.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/05/11/exm2.png)

**Input:** n = 3, queries = \[\[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]

**Output:** 17

**Explanation:** The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 17.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i].length == 3`
*   <code>0 <= type<sub>i</sub> <= 1</code>
*   <code>0 <= index<sub>i</sub> < n</code>
*   <code>0 <= val<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun matrixSumQueries(n: Int, queries: Array<IntArray>): Long {
        val queriedRow = BooleanArray(n)
        val queriedCol = BooleanArray(n)
        var sum: Long = 0
        var remainingRows = n
        var remainingCols = n
        for (i in queries.indices.reversed()) {
            val type = queries[i][0]
            val index = queries[i][1]
            val value = queries[i][2]
            if (type == 0) {
                if (queriedRow[index]) {
                    continue
                }
                sum += (value * remainingCols).toLong()
                remainingRows--
                queriedRow[index] = true
            } else {
                if (queriedCol[index]) {
                    continue
                }
                sum += (value * remainingRows).toLong()
                remainingCols--
                queriedCol[index] = true
            }
        }
        return sum
    }
}
```