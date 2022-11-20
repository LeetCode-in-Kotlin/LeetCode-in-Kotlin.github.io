[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 363\. Max Sum of Rectangle No Larger Than K

Hard

Given an `m x n` matrix `matrix` and an integer `k`, return _the max sum of a rectangle in the matrix such that its sum is no larger than_ `k`.

It is **guaranteed** that there will be a rectangle with a sum no larger than `k`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/18/sum-grid.jpg)

**Input:** matrix = \[\[1,0,1],[0,-2,3]], k = 2

**Output:** 2

**Explanation:** Because the sum of the blue rectangle [[0, 1], [-2, 3]] is 2, and 2 is the max number no larger than k (k = 2).

**Example 2:**

**Input:** matrix = \[\[2,2,-1]], k = 3

**Output:** 3

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 100`
*   `-100 <= matrix[i][j] <= 100`
*   <code>-10<sup>5</sup> <= k <= 10<sup>5</sup></code>

**Follow up:** What if the number of rows is much larger than the number of columns?

## Solution

```kotlin
class Solution {
    fun maxSumSubmatrix(matrix: Array<IntArray>, k: Int): Int {
        if (matrix.size == 0 || matrix[0].size == 0) {
            return 0
        }
        val row = matrix.size
        val col = matrix[0].size
        var res = Int.MIN_VALUE
        for (i in 0 until col) {
            val sum = IntArray(row)
            for (j in i until col) {
                for (r in 0 until row) {
                    sum[r] += matrix[r][j]
                }
                var curr = 0
                var max = sum[0]
                for (n in sum) {
                    curr = Math.max(n, curr + n)
                    max = Math.max(curr, max)
                    if (max == k) {
                        return max
                    }
                }
                if (max < k) {
                    res = Math.max(max, res)
                } else {
                    for (a in 0 until row) {
                        var currSum = 0
                        for (b in a until row) {
                            currSum += sum[b]
                            if (currSum <= k) {
                                res = Math.max(currSum, res)
                            }
                        }
                    }
                    if (res == k) {
                        return res
                    }
                }
            }
        }
        return res
    }
}
```