[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 547\. Number of Provinces

Medium

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the <code>i<sup>th</sup></code> city and the <code>j<sup>th</sup></code> city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return _the total number of **provinces**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

**Input:** isConnected = \[\[1,1,0],[1,1,0],[0,0,1]]

**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

**Input:** isConnected = \[\[1,0,0],[0,1,0],[0,0,1]]

**Output:** 3

**Constraints:**

*   `1 <= n <= 200`
*   `n == isConnected.length`
*   `n == isConnected[i].length`
*   `isConnected[i][j]` is `1` or `0`.
*   `isConnected[i][i] == 1`
*   `isConnected[i][j] == isConnected[j][i]`

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun findCircleNum(arr: Array<IntArray>): Int {
        val parent = IntArray(arr.size)
        Arrays.fill(parent, -1)
        var ans = 0
        for (i in 0 until arr.size - 1) {
            for (j in i + 1 until arr[i].size) {
                if (arr[i][j] == 1) {
                    ans += union(i, j, parent)
                }
            }
        }
        return arr.size - ans
    }

    private fun union(a: Int, b: Int, arr: IntArray): Int {
        val ga = find(a, arr)
        val gb = find(b, arr)
        if (ga != gb) {
            arr[gb] = ga
            return 1
        }
        return 0
    }

    private fun find(a: Int, arr: IntArray): Int {
        if (arr[a] == -1) {
            return a
        }
        arr[a] = find(arr[a], arr)
        return arr[a]
    }
}
```