[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2033\. Minimum Operations to Make a Uni-Value Grid

Medium

You are given a 2D integer `grid` of size `m x n` and an integer `x`. In one operation, you can **add** `x` to or **subtract** `x` from any element in the `grid`.

A **uni-value grid** is a grid where all the elements of it are equal.

Return _the **minimum** number of operations to make the grid **uni-value**_. If it is not possible, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt.png)

**Input:** grid = \[\[2,4],[6,8]], x = 2

**Output:** 4

**Explanation:** We can make every element equal to 4 by doing the following: 

- Add x to 2 once. 

- Subtract x from 6 once. 

- Subtract x from 8 twice. 
  
A total of 4 operations were used.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-1.png)

**Input:** grid = \[\[1,5],[2,3]], x = 1

**Output:** 5

**Explanation:** We can make every element equal to 3.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-2.png)

**Input:** grid = \[\[1,2],[3,4]], x = 2

**Output:** -1

**Explanation:** It is impossible to make every element equal.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= x, grid[i][j] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun minOperations(grid: Array<IntArray>, x: Int): Int {
        val arr = IntArray(grid.size * grid[0].size)
        var k = 0
        for (ints in grid) {
            for (j in grid[0].indices) {
                arr[k] = ints[j]
                k++
            }
        }
        arr.sort()
        val target = arr[arr.size / 2]
        var res = 0
        for (i in arr.indices) {
            res += if (i < arr.size / 2) {
                val rem = target - arr[i]
                if (rem % x != 0) {
                    return -1
                }
                rem / x
            } else {
                val rem = arr[i] - target
                if (rem % x != 0) {
                    return -1
                }
                rem / x
            }
        }
        return res
    }
}
```