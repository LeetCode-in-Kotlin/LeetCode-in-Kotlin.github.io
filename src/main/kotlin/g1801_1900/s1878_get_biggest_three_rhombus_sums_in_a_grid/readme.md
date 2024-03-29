[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1878\. Get Biggest Three Rhombus Sums in a Grid

Medium

You are given an `m x n` integer matrix `grid`.

A **rhombus sum** is the sum of the elements that form **the** **border** of a regular rhombus shape in `grid`. The rhombus must have the shape of a square rotated 45 degrees with each of the corners centered in a grid cell. Below is an image of four valid rhombus shapes with the corresponding colored cells that should be included in each **rhombus sum**:

![](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-desc-2.png)

Note that the rhombus can have an area of 0, which is depicted by the purple rhombus in the bottom right corner.

Return _the biggest three **distinct rhombus sums** in the_ `grid` _in **descending order**__. If there are less than three distinct values, return all of them_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex1.png)

**Input:** grid = \[\[3,4,5,1,3],[3,3,4,2,3],[20,30,200,40,10],[1,5,5,4,1],[4,3,2,2,5]]

**Output:** [228,216,211]

**Explanation:** The rhombus shapes for the three biggest distinct rhombus sums are depicted above. 

- Blue: 20 + 3 + 200 + 5 = 228 

- Red: 200 + 2 + 10 + 4 = 216 

- Green: 5 + 200 + 4 + 2 = 211

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/23/pc73-q4-ex2.png)

**Input:** grid = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [20,9,8]

**Explanation:** The rhombus shapes for the three biggest distinct rhombus sums are depicted above. 

- Blue: 4 + 2 + 6 + 8 = 20 

- Red: 9 (area 0 rhombus in the bottom right corner) 

- Green: 8 (area 0 rhombus in the bottom middle)

**Example 3:**

**Input:** grid = \[\[7,7,7]]

**Output:** [7]

**Explanation:** All three possible rhombus sums are the same, so return [7].

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 50`
*   <code>1 <= grid[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun getBiggestThree(grid: Array<IntArray>): IntArray {
        val capicity = 3
        val minHeap = PriorityQueue<Int>()
        val m = grid.size
        val n = grid[0].size
        val preSum = Array(m) { Array(n) { IntArray(2) } }
        val maxLen = Math.min(m, n) / 2
        for (r in 0 until m) {
            for (c in 0 until n) {
                addToMinHeap(minHeap, grid[r][c], capicity)
                preSum[r][c][0] += if (valid(m, n, r - 1, c - 1)) grid[r][c] + preSum[r - 1][c - 1][0] else grid[r][c]
                preSum[r][c][1] += if (valid(m, n, r - 1, c + 1)) grid[r][c] + preSum[r - 1][c + 1][1] else grid[r][c]
            }
        }
        for (r in 0 until m) {
            for (c in 0 until n) {
                for (l in 1..maxLen) {
                    if (!valid(m, n, r - l, c - l) ||
                        !valid(m, n, r - l, c + l) ||
                        !valid(m, n, r - 2 * l, c)
                    ) {
                        break
                    }
                    var rhombus = preSum[r][c][0] - preSum[r - l][c - l][0]
                    rhombus += preSum[r][c][1] - preSum[r - l][c + l][1]
                    rhombus += preSum[r - l][c - l][1] - preSum[r - 2 * l][c][1]
                    rhombus += preSum[r - l][c + l][0] - preSum[r - 2 * l][c][0]
                    rhombus += -grid[r][c] + grid[r - 2 * l][c]
                    addToMinHeap(minHeap, rhombus, capicity)
                }
            }
        }
        val size = minHeap.size
        val res = IntArray(size)
        for (i in size - 1 downTo 0) {
            res[i] = minHeap.poll()
        }
        return res
    }

    private fun addToMinHeap(minHeap: PriorityQueue<Int>, num: Int, capicity: Int) {
        if (minHeap.isEmpty() || minHeap.size < capicity && !minHeap.contains(num)) {
            minHeap.offer(num)
        } else {
            if (num > minHeap.peek() && !minHeap.contains(num)) {
                minHeap.poll()
                minHeap.offer(num)
            }
        }
    }

    private fun valid(m: Int, n: Int, r: Int, c: Int): Boolean {
        return 0 <= r && r < m && 0 <= c && c < n
    }
}
```