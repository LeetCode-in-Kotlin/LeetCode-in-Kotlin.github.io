[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3462\. Maximum Sum With at Most K Elements

Medium

You are given a 2D integer matrix `grid` of size `n x m`, an integer array `limits` of length `n`, and an integer `k`. The task is to find the **maximum sum** of **at most** `k` elements from the matrix `grid` such that:

*   The number of elements taken from the <code>i<sup>th</sup></code> row of `grid` does not exceed `limits[i]`.
    

Return the **maximum sum**.

**Example 1:**

**Input:** grid = \[\[1,2],[3,4]], limits = [1,2], k = 2

**Output:** 7

**Explanation:**

*   From the second row, we can take at most 2 elements. The elements taken are 4 and 3.
*   The maximum possible sum of at most 2 selected elements is `4 + 3 = 7`.

**Example 2:**

**Input:** grid = \[\[5,3,7],[8,2,6]], limits = [2,2], k = 3

**Output:** 21

**Explanation:**

*   From the first row, we can take at most 2 elements. The element taken is 7.
*   From the second row, we can take at most 2 elements. The elements taken are 8 and 6.
*   The maximum possible sum of at most 3 selected elements is `7 + 8 + 6 = 21`.

**Constraints:**

*   `n == grid.length == limits.length`
*   `m == grid[i].length`
*   `1 <= n, m <= 500`
*   <code>0 <= grid[i][j] <= 10<sup>5</sup></code>
*   `0 <= limits[i] <= m`
*   `0 <= k <= min(n * m, sum(limits))`

## Solution

```kotlin
class Solution {
    fun maxSum(grid: Array<IntArray>, limits: IntArray, k: Int): Long {
        var l = 0
        for (i in limits.indices) {
            l += limits[i]
        }
        val dp = IntArray(l)
        var a = 0
        for (i in grid.indices) {
            val lim = limits[i]
            grid[i].sort()
            for (j in grid[i].size - lim..<grid[i].size) {
                dp[a] = grid[i][j]
                a++
            }
        }
        dp.sort()
        var sum = 0L
        for (i in l - 1 downTo l - k) {
            sum += dp[i].toLong()
        }
        return sum
    }
}
```