[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1074\. Number of Submatrices That Sum to Target

Hard

Given a `matrix` and a `target`, return the number of non-empty submatrices that sum to target.

A submatrix `x1, y1, x2, y2` is the set of all cells `matrix[x][y]` with `x1 <= x <= x2` and `y1 <= y <= y2`.

Two submatrices `(x1, y1, x2, y2)` and `(x1', y1', x2', y2')` are different if they have some coordinate that is different: for example, if `x1 != x1'`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

**Input:** matrix = \[\[0,1,0],[1,1,1],[0,1,0]], target = 0

**Output:** 4

**Explanation:** The four 1x1 submatrices that only contain 0.

**Example 2:**

**Input:** matrix = \[\[1,-1],[-1,1]], target = 0

**Output:** 5

**Explanation:** The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.

**Example 3:**

**Input:** matrix = \[\[904]], target = 0

**Output:** 0

**Constraints:**

*   `1 <= matrix.length <= 100`
*   `1 <= matrix[0].length <= 100`
*   `-1000 <= matrix[i] <= 1000`
*   `-10^8 <= target <= 10^8`

## Solution

```kotlin
class Solution {
    fun numSubmatrixSumTarget(matrix: Array<IntArray>, target: Int): Int {
        val rows = matrix.size
        val columns = matrix[0].size
        for (i in 0 until rows) {
            for (j in 1 until columns) {
                matrix[i][j] += matrix[i][j - 1]
            }
        }
        val sumMap: HashMap<Int, Int> = HashMap()
        var cur: Int
        var res = 0
        for (i in 0 until columns) {
            for (j in i until columns) {
                sumMap.clear()
                sumMap[0] = 1
                cur = 0
                for (ints in matrix) {
                    cur += ints[j] - if (i > 0) ints[i - 1] else 0
                    res += sumMap.getOrDefault(cur - target, 0)
                    sumMap[cur] = sumMap.getOrDefault(cur, 0) + 1
                }
            }
        }
        return res
    }
}
```