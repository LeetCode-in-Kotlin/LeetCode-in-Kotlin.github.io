[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3417\. Zigzag Grid Traversal With Skip

Easy

You are given an `m x n` 2D array `grid` of **positive** integers.

Your task is to traverse `grid` in a **zigzag** pattern while skipping every **alternate** cell.

Zigzag pattern traversal is defined as following the below actions:

*   Start at the top-left cell `(0, 0)`.
*   Move _right_ within a row until the end of the row is reached.
*   Drop down to the next row, then traverse _left_ until the beginning of the row is reached.
*   Continue **alternating** between right and left traversal until every row has been traversed.

**Note** that you **must skip** every _alternate_ cell during the traversal.

Return an array of integers `result` containing, **in order**, the value of the cells visited during the zigzag traversal with skips.

**Example 1:**

**Input:** grid = \[\[1,2],[3,4]]

**Output:** [1,4]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/11/23/4012_example0.png)**

**Example 2:**

**Input:** grid = \[\[2,1],[2,1],[2,1]]

**Output:** [2,1,2]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/23/4012_example1.png)

**Example 3:**

**Input:** grid = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,3,5,7,9]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/23/4012_example2.png)

**Constraints:**

*   `2 <= n == grid.length <= 50`
*   `2 <= m == grid[i].length <= 50`
*   `1 <= grid[i][j] <= 2500`

## Solution

```kotlin
class Solution {
    fun zigzagTraversal(grid: Array<IntArray>): List<Int> {
        val ans: MutableList<Int> = ArrayList<Int>()
        val m = grid.size
        val n = grid[0].size
        var i = 0
        var flag = true
        var skip = false
        while (i < m) {
            if (flag) {
                for (j in 0..<n) {
                    if (!skip) {
                        ans.add(grid[i][j])
                    }
                    skip = !skip
                }
            } else {
                for (j in n - 1 downTo 0) {
                    if (!skip) {
                        ans.add(grid[i][j])
                    }
                    skip = !skip
                }
            }
            flag = !flag
            i++
        }
        return ans
    }
}
```