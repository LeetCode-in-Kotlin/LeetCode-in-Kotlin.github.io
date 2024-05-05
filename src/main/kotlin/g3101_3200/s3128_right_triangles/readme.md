[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3128\. Right Triangles

Medium

You are given a 2D boolean matrix `grid`.

Return an integer that is the number of **right triangles** that can be made with the 3 elements of `grid` such that **all** of them have a value of 1.

**Note:**

*   A collection of 3 elements of `grid` is a **right triangle** if one of its elements is in the **same row** with another element and in the **same column** with the third element. The 3 elements do not have to be next to each other.

**Example 1:**

0 **1** 0

0 **1 1**

0 1 0

0 1 0

0 **1 1**

0 **1** 0

**Input:** grid = \[\[0,1,0],[0,1,1],[0,1,0]]

**Output:** 2

**Explanation:**

There are two right triangles.

**Example 2:**

1 0 0 0

0 1 0 1

1 0 0 0

**Input:** grid = \[\[1,0,0,0],[0,1,0,1],[1,0,0,0]]

**Output:** 0

**Explanation:**

There are no right triangles.

**Example 3:**

**1** 0 **1**

**1** 0 0

1 0 0

**1** 0 **1**

1 0 0

**1** 0 0

**Input:** grid = \[\[1,0,1],[1,0,0],[1,0,0]]

**Output: **2

**Explanation:**

There are two right triangles.

**Constraints:**

*   `1 <= grid.length <= 1000`
*   `1 <= grid[i].length <= 1000`
*   `0 <= grid[i][j] <= 1`

## Solution

```kotlin
class Solution {
    fun numberOfRightTriangles(grid: Array<IntArray>): Long {
        val n = grid.size
        val m = grid[0].size
        val columns = IntArray(n)
        val rows = IntArray(m)
        var sum: Long = 0
        for (i in 0 until n) {
            for (j in 0 until m) {
                columns[i] += grid[i][j]
                rows[j] += grid[i][j]
            }
        }
        for (i in 0 until n) {
            for (j in 0 until m) {
                sum += grid[i][j].toLong() * (rows[j] - 1) * (columns[i] - 1)
            }
        }
        return sum
    }
}
```