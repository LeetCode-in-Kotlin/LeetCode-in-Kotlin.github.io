[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 120\. Triangle

Medium

Given a `triangle` array, return _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

**Input:** triangle = \[\[2],[3,4],[6,5,7],[4,1,8,3]]

**Output:** 11

**Explanation:** The triangle looks like: 
    
<ins>2</ins> 

<ins>3</ins> 4 

6 <ins>5</ins> 7 

4 <ins>1</ins> 8 3 

The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

**Example 2:**

**Input:** triangle = \[\[-10]]

**Output:** -10

**Constraints:**

*   `1 <= triangle.length <= 200`
*   `triangle[0].length == 1`
*   `triangle[i].length == triangle[i - 1].length + 1`
*   <code>-10<sup>4</sup> <= triangle[i][j] <= 10<sup>4</sup></code>

**Follow up:** Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun minimumTotal(triangle: List<List<Int>>): Int {
        if (triangle.isEmpty()) {
            return 0
        }
        val dp = Array(triangle.size) { IntArray(triangle[triangle.size - 1].size) }
        for (temp in dp) {
            Arrays.fill(temp, -10001)
        }
        return dfs(triangle, dp, 0, 0)
    }

    private fun dfs(triangle: List<List<Int>>, dp: Array<IntArray>, row: Int, col: Int): Int {
        if (row >= triangle.size) {
            return 0
        }
        if (dp[row][col] != -10001) {
            return dp[row][col]
        }
        val sum = (
            triangle[row][col] +
                Math.min(
                    dfs(triangle, dp, row + 1, col),
                    dfs(triangle, dp, row + 1, col + 1)
                )
            )
        dp[row][col] = sum
        return sum
    }
}
```