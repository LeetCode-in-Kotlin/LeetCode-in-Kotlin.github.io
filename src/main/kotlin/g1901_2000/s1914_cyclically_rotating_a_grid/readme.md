[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1914\. Cyclically Rotating a Grid

Medium

You are given an `m x n` integer matrix `grid`, where `m` and `n` are both **even** integers, and an integer `k`.

The matrix is composed of several layers, which is shown in the below image, where each color is its own layer:

![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid.png)

A cyclic rotation of the matrix is done by cyclically rotating **each layer** in the matrix. To cyclically rotate a layer once, each element in the layer will take the place of the adjacent element in the **counter-clockwise** direction. An example rotation is shown below:

![](https://assets.leetcode.com/uploads/2021/06/22/explanation_grid.jpg)

Return _the matrix after applying_ `k` _cyclic rotations to it_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/19/rod2.png)

**Input:** grid = \[\[40,10],[30,20]], k = 1

**Output:** [[10,20],[40,30]]

**Explanation:** The figures above represent the grid at every state.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid5.png)** **![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid6.png)** **![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid7.png)**

**Input:** grid = \[\[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2

**Output:** [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]

**Explanation:** The figures above represent the grid at every state.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 50`
*   Both `m` and `n` are **even** integers.
*   `1 <= grid[i][j] <= 5000`
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun rotateGrid(grid: Array<IntArray>, k: Int): Array<IntArray> {
        rotateInternal(grid, 0, grid[0].size - 1, 0, grid.size - 1, k)
        return grid
    }

    private fun rotateInternal(grid: Array<IntArray>, left: Int, right: Int, up: Int, bottom: Int, k: Int) {
        if (left > right || up > bottom) {
            return
        }
        val loopLen = (right - left + 1) * 2 + (bottom - up + 1) * 2 - 4
        val realK = k % loopLen
        if (realK != 0) {
            rotateLayer(grid, left, right, up, bottom, realK)
        }
        rotateInternal(grid, left + 1, right - 1, up + 1, bottom - 1, k)
    }

    private fun rotateLayer(grid: Array<IntArray>, left: Int, right: Int, up: Int, bottom: Int, k: Int) {
        val startPoint = intArrayOf(up, left)
        val loopLen = (right - left + 1) * 2 + (bottom - up + 1) * 2 - 4
        val arr = IntArray(loopLen)
        var idx = 0
        var currPoint: IntArray? = startPoint
        var startPointAfterRotation: IntArray? = null
        while (idx < arr.size) {
            arr[idx] = grid[currPoint!![0]][currPoint[1]]
            idx++
            currPoint = getNextPosCC(left, right, up, bottom, currPoint)
            if (idx == k) {
                startPointAfterRotation = currPoint
            }
        }
        idx = 0
        currPoint = startPointAfterRotation
        if (currPoint != null) {
            while (idx < arr.size) {
                grid[currPoint!![0]][currPoint[1]] = arr[idx]
                idx++
                currPoint = getNextPosCC(left, right, up, bottom, currPoint)
            }
        }
    }

    private fun getNextPosCC(left: Int, right: Int, up: Int, bottom: Int, curr: IntArray?): IntArray {
        val x = curr!![0]
        val y = curr[1]
        return if (x == up && y > left) {
            intArrayOf(x, y - 1)
        } else if (y == left && x < bottom) {
            intArrayOf(x + 1, y)
        } else if (x == bottom && y < right) {
            intArrayOf(x, y + 1)
        } else {
            intArrayOf(x - 1, y)
        }
    }
}
```