[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1007\. Minimum Domino Rotations For Equal Row

Medium

In a row of dominoes, `tops[i]` and `bottoms[i]` represent the top and bottom halves of the <code>i<sup>th</sup></code> domino. (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the <code>i<sup>th</sup></code> domino, so that `tops[i]` and `bottoms[i]` swap values.

Return the minimum number of rotations so that all the values in `tops` are the same, or all the values in `bottoms` are the same.

If it cannot be done, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/14/domino.png)

**Input:** tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]

**Output:** 2

**Explanation:** The first figure represents the dominoes as given by tops and bottoms: before we do any rotations. If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.

**Example 2:**

**Input:** tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]

**Output:** -1

**Explanation:** In this case, it is not possible to rotate the dominoes to make one row of values equal.

**Constraints:**

*   <code>2 <= tops.length <= 2 * 10<sup>4</sup></code>
*   `bottoms.length == tops.length`
*   `1 <= tops[i], bottoms[i] <= 6`

## Solution

```kotlin
class Solution {
    fun minDominoRotations(tops: IntArray, bottoms: IntArray): Int {
        val top = tops[0]
        var tCount = 0
        var bCount = 0
        val tSwaps: Int
        val bSwaps: Int
        var swaps = 0
        var valid = true
        for (i in tops.indices) {
            if (tops[i] == top) {
                tCount++
            }
            if (bottoms[i] == top) {
                bCount++
            }
            if (tops[i] != top && bottoms[i] != top) {
                valid = false
                swaps = -1
                break
            }
        }
        if (valid) {
            tSwaps = tops.size - tCount
            bSwaps = bottoms.size - bCount
            swaps = Math.min(tSwaps, bSwaps)
        }
        val bottom = bottoms[0]
        var tCount1 = 0
        var bCount1 = 0
        val tSwaps1: Int
        val bSwaps1: Int
        var swaps1 = 0
        var valid1 = true
        for (i in bottoms.indices) {
            if (tops[i] == bottom) {
                tCount1++
            }
            if (bottoms[i] == bottom) {
                bCount1++
            }
            if (tops[i] != bottom && bottoms[i] != bottom) {
                valid1 = false
                swaps1 = -1
                break
            }
        }
        if (valid1) {
            tSwaps1 = tops.size - tCount1
            bSwaps1 = bottoms.size - bCount1
            swaps1 = Math.min(tSwaps1, bSwaps1)
        }
        val ans = IntArray(2)
        if (swaps1 < swaps) {
            ans[0] = swaps1
            ans[1] = swaps
        } else {
            ans[0] = swaps
            ans[1] = swaps1
        }
        return if (ans[0] != -1) {
            ans[0]
        } else {
            ans[1]
        }
    }
}
```