[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2482\. Difference Between Ones and Zeros in Row and Column

Medium

You are given a **0-indexed** `m x n` binary matrix `grid`.

A **0-indexed** `m x n` difference matrix `diff` is created with the following procedure:

*   Let the number of ones in the <code>i<sup>th</sup></code> row be <code>onesRow<sub>i</sub></code>.
*   Let the number of ones in the <code>j<sup>th</sup></code> column be <code>onesCol<sub>j</sub></code>.
*   Let the number of zeros in the <code>i<sup>th</sup></code> row be <code>zerosRow<sub>i</sub></code>.
*   Let the number of zeros in the <code>j<sup>th</sup></code> column be <code>zerosCol<sub>j</sub></code>.
*   <code>diff[i][j] = onesRow<sub>i</sub> + onesCol<sub>j</sub> - zerosRow<sub>i</sub> - zerosCol<sub>j</sub></code>

Return _the difference matrix_ `diff`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171729-5.png)

**Input:** grid = \[\[0,1,1],[1,0,1],[0,0,1]]

**Output:** [[0,0,4],[0,0,4],[-2,-2,2]]

**Explanation:**
- diff[0][0] = <code>onesRow<sub>0</sub> + onesCol<sub>0</sub> - zerosRow<sub>0</sub> - zerosCol<sub>0</sub></code> = 2 + 1 - 1 - 2 = 0 
- diff[0][1] = <code>onesRow<sub>0</sub> + onesCol<sub>1</sub> - zerosRow<sub>0</sub> - zerosCol<sub>1</sub></code> = 2 + 1 - 1 - 2 = 0 
- diff[0][2] = <code>onesRow<sub>0</sub> + onesCol<sub>2</sub> - zerosRow<sub>0</sub> - zerosCol<sub>2</sub></code> = 2 + 3 - 1 - 0 = 4 
- diff[1][0] = <code>onesRow<sub>1</sub> + onesCol<sub>0</sub> - zerosRow<sub>1</sub> - zerosCol<sub>0</sub></code> = 2 + 1 - 1 - 2 = 0 
- diff[1][1] = <code>onesRow<sub>1</sub> + onesCol<sub>1</sub> - zerosRow<sub>1</sub> - zerosCol<sub>1</sub></code> = 2 + 1 - 1 - 2 = 0 
- diff[1][2] = <code>onesRow<sub>1</sub> + onesCol<sub>2</sub> - zerosRow<sub>1</sub> - zerosCol<sub>2</sub></code> = 2 + 3 - 1 - 0 = 4 
- diff[2][0] = <code>onesRow<sub>2</sub> + onesCol<sub>0</sub> - zerosRow<sub>2</sub> - zerosCol<sub>0</sub></code> = 1 + 1 - 2 - 2 = -2 
- diff[2][1] = <code>onesRow<sub>2</sub> + onesCol<sub>1</sub> - zerosRow<sub>2</sub> - zerosCol<sub>1</sub></code> = 1 + 1 - 2 - 2 = -2 
- diff[2][2] = <code>onesRow<sub>2</sub> + onesCol<sub>2</sub> - zerosRow<sub>2</sub> - zerosCol<sub>2</sub></code> = 1 + 3 - 2 - 0 = 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171747-6.png)

**Input:** grid = \[\[1,1,1],[1,1,1]]

**Output:** [[5,5,5],[5,5,5]]

**Explanation:** 
- diff[0][0] = onesRow<sub>0</sub> + onesCol<sub>0</sub> - zerosRow<sub>0</sub> - zerosCol<sub>0</sub> = 3 + 2 - 0 - 0 = 5 
- diff[0][1] = onesRow<sub>0</sub> + onesCol<sub>1</sub> - zerosRow<sub>0</sub> - zerosCol<sub>1</sub> = 3 + 2 - 0 - 0 = 5 
- diff[0][2] = onesRow<sub>0</sub> + onesCol<sub>2</sub> - zerosRow<sub>0</sub> - zerosCol<sub>2</sub> = 3 + 2 - 0 - 0 = 5 
- diff[1][0] = onesRow<sub>1</sub> + onesCol<sub>0</sub> - zerosRow<sub>1</sub> - zerosCol<sub>0</sub> = 3 + 2 - 0 - 0 = 5 
- diff[1][1] = onesRow<sub>1</sub> + onesCol<sub>1</sub> - zerosRow<sub>1</sub> - zerosCol<sub>1</sub> = 3 + 2 - 0 - 0 = 5 
- diff[1][2] = onesRow<sub>1</sub> + onesCol<sub>2</sub> - zerosRow<sub>1</sub> - zerosCol<sub>2</sub> = 3 + 2 - 0 - 0 = 5

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun onesMinusZeros(grid: Array<IntArray>): Array<IntArray> {
        val rowOne = IntArray(grid.size)
        val colOne = IntArray(grid[0].size)
        val m = grid.size
        val n = grid[0].size
        for (i in 0 until m) {
            var c = 0
            for (j in 0 until n) {
                if (grid[i][j] == 1) {
                    c++
                }
            }
            rowOne[i] = c
        }
        for (i in 0 until n) {
            var c = 0
            for (ints in grid) {
                if (ints[i] == 1) {
                    c++
                }
            }
            colOne[i] = c
        }
        for (i in 0 until m) {
            for (j in 0 until n) {
                grid[i][j] = rowOne[i] + colOne[j] - (m - rowOne[i]) - (n - colOne[j])
            }
        }
        return grid
    }
}
```