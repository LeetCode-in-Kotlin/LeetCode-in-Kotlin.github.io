[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3071\. Minimum Operations to Write the Letter Y on a Grid

Medium

You are given a **0-indexed** `n x n` grid where `n` is odd, and `grid[r][c]` is `0`, `1`, or `2`.

We say that a cell belongs to the Letter **Y** if it belongs to one of the following:

*   The diagonal starting at the top-left cell and ending at the center cell of the grid.
*   The diagonal starting at the top-right cell and ending at the center cell of the grid.
*   The vertical line starting at the center cell and ending at the bottom border of the grid.

The Letter **Y** is written on the grid if and only if:

*   All values at cells belonging to the Y are equal.
*   All values at cells not belonging to the Y are equal.
*   The values at cells belonging to the Y are different from the values at cells not belonging to the Y.

Return _the **minimum** number of operations needed to write the letter Y on the grid given that in one operation you can change the value at any cell to_ `0`_,_ `1`_,_ _or_ `2`_._

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/01/22/y2.png)

**Input:** grid = \[\[1,2,2],[1,1,0],[0,1,0]]

**Output:** 3

**Explanation:** We can write Y on the grid by applying the changes highlighted in blue in the image above. After the operations, all cells that belong to Y, denoted in bold, have the same value of 1 while those that do not belong to Y are equal to 0. It can be shown that 3 is the minimum number of operations needed to write Y on the grid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/01/22/y3.png)

**Input:** grid = \[\[0,1,0,1,0],[2,1,0,1,2],[2,2,2,0,1],[2,2,2,2,2],[2,1,2,2,2]]

**Output:** 12

**Explanation:** We can write Y on the grid by applying the changes highlighted in blue in the image above. After the operations, all cells that belong to Y, denoted in bold, have the same value of 0 while those that do not belong to Y are equal to 2. It can be shown that 12 is the minimum number of operations needed to write Y on the grid.

**Constraints:**

*   `3 <= n <= 49`
*   `n == grid.length == grid[i].length`
*   `0 <= grid[i][j] <= 2`
*   `n` is odd.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumOperationsToWriteY(arr: Array<IntArray>): Int {
        val n = arr.size
        val cnt1 = IntArray(3)
        val cnt2 = IntArray(3)
        val x = n / 2
        val y = n / 2
        for (j in x until n) {
            cnt1[arr[j][y]]++
            arr[j][y] = 3
        }
        for (j in x downTo 0) {
            if (arr[j][j] != 3) {
                cnt1[arr[j][j]]++
            }
            arr[j][j] = 3
        }
        for (j in x downTo 0) {
            if (arr[j][n - 1 - j] != 3) {
                cnt1[arr[j][n - 1 - j]]++
            }
            arr[j][n - 1 - j] = 3
        }
        for (ints in arr) {
            for (j in 0 until n) {
                if (ints[j] != 3) {
                    cnt2[ints[j]]++
                }
            }
        }
        val s1 = cnt1[0] + cnt1[1] + cnt1[2]
        val s2 = cnt2[0] + cnt2[1] + cnt2[2]
        var min = Int.MAX_VALUE
        for (i in 0..2) {
            for (j in 0..2) {
                if (i != j) {
                    min = min((s1 - cnt1[i] + s2 - cnt2[j]), min)
                }
            }
        }
        return min
    }
}
```