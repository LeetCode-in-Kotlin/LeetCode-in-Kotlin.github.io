[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1139\. Largest 1-Bordered Square

Medium

Given a 2D `grid` of `0`s and `1`s, return the number of elements in the largest **square** subgrid that has all `1`s on its **border**, or `0` if such a subgrid doesn't exist in the `grid`.

**Example 1:**

**Input:** grid = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** 9

**Example 2:**

**Input:** grid = \[\[1,1,0,0]]

**Output:** 1

**Constraints:**

*   `1 <= grid.length <= 100`
*   `1 <= grid[0].length <= 100`
*   `grid[i][j]` is `0` or `1`

## Solution

```kotlin
class Solution {
    fun largest1BorderedSquare(grid: Array<IntArray>): Int {
        val rightToLeft = arrayOfNulls<IntArray>(grid.size)
        val bottomToUp = arrayOfNulls<IntArray>(grid.size)
        for (i in grid.indices) {
            rightToLeft[i] = grid[i].clone()
            bottomToUp[i] = grid[i].clone()
        }
        val row = grid.size
        val col = grid[0].size
        for (i in 0 until row) {
            for (j in col - 2 downTo 0) {
                if (grid[i][j] == 1) {
                    rightToLeft[i]!![j] = rightToLeft[i]!![j + 1] + 1
                }
            }
        }
        for (j in 0 until col) {
            for (i in row - 2 downTo 0) {
                if (grid[i][j] == 1) {
                    bottomToUp[i]!![j] = bottomToUp[i + 1]!![j] + 1
                }
            }
        }
        var res = 0
        for (i in 0 until row) {
            for (j in 0 until col) {
                val curLen = rightToLeft[i]!![j]
                for (k in curLen downTo 1) {
                    if (bottomToUp[i]!![j] >= k && rightToLeft[i + k - 1]!![j] >= k &&
                        bottomToUp[i]!![j + k - 1] >= k
                    ) {
                        if (k > res) {
                            res = k
                        }
                        break
                    }
                }
            }
        }
        return res * res
    }
}
```