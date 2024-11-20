[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1292\. Maximum Side Length of a Square with Sum Less than or Equal to Threshold

Medium

Given a `m x n` matrix `mat` and an integer `threshold`, return _the maximum side-length of a square with a sum less than or equal to_ `threshold` _or return_ `0` _if there is no such square_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/05/e1.png)

**Input:** mat = \[\[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4

**Output:** 2

**Explanation:** The maximum side length of square with sum less than 4 is 2 as shown.

**Example 2:**

**Input:** mat = \[\[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1

**Output:** 0

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 300`
*   <code>0 <= mat[i][j] <= 10<sup>4</sup></code>
*   <code>0 <= threshold <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun maxSideLength(mat: Array<IntArray>, threshold: Int): Int {
        val m = mat.size
        val n = mat[0].size
        val prefix = Array(m) { IntArray(n) }
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (i == 0 && j == 0) {
                    prefix[i][j] = mat[i][j]
                } else if (i == 0) {
                    prefix[i][j] = mat[i][j] + prefix[0][j - 1]
                } else if (j == 0) {
                    prefix[i][j] = mat[i][j] + prefix[i - 1][0]
                } else {
                    prefix[i][j] = mat[i][j] + prefix[i][j - 1] + prefix[i - 1][j] - prefix[i - 1][j - 1]
                }
            }
        }
        var low = 1
        var high = Math.min(m, n)
        var ans = 0
        while (low <= high) {
            val mid = (low + high) / 2
            if (min(mid, prefix) > threshold) {
                high = mid - 1
            } else {
                ans = mid
                low = mid + 1
            }
        }
        return ans
    }

    fun min(length: Int, prefix: Array<IntArray>): Int {
        var min = 0
        for (i in length - 1 until prefix.size) {
            for (j in length - 1 until prefix[0].size) {
                min = if (i == length - 1 && j == length - 1) {
                    prefix[i][j]
                } else if (i - length < 0) {
                    Math.min(min, prefix[i][j] - prefix[i][j - length])
                } else if (j - length < 0) {
                    Math.min(min, prefix[i][j] - prefix[i - length][j])
                } else {
                    Math.min(
                        min,
                        (
                            prefix[i][j] -
                                prefix[i][j - length] -
                                prefix[i - length][j]
                            ) +
                            prefix[i - length][j - length],
                    )
                }
            }
        }
        return min
    }
}
```