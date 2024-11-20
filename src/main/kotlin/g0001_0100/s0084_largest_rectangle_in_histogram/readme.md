[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 84\. Largest Rectangle in Histogram

Hard

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights = [2,1,5,6,2,3]

**Output:** 10

**Explanation:** The above is a histogram where width of each bar is 1. 

The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = [2,4]

**Output:** 4

**Constraints:**

*   <code>1 <= heights.length <= 10<sup>5</sup></code>
*   <code>0 <= heights[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun largestRectangleArea(heights: IntArray): Int {
        return largestArea(heights, 0, heights.size)
    }

    private fun largestArea(a: IntArray, start: Int, limit: Int): Int {
        if (a.isEmpty()) {
            return 0
        }
        if (start == limit) {
            return 0
        }
        if (limit - start == 1) {
            return a[start]
        }
        if (limit - start == 2) {
            val maxOfTwoBars = Math.max(a[start], a[start + 1])
            val areaFromTwo = Math.min(a[start], a[start + 1]) * 2
            return Math.max(maxOfTwoBars, areaFromTwo)
        }
        return if (checkIfSorted(a, start, limit)) {
            var maxWhenSorted = 0
            for (i in start until limit) {
                if (a[i] * (limit - i) > maxWhenSorted) {
                    maxWhenSorted = a[i] * (limit - i)
                }
            }
            maxWhenSorted
        } else {
            val minInd = findMinInArray(a, start, limit)
            maxOfThreeNums(
                largestArea(a, start, minInd),
                a[minInd] * (limit - start),
                largestArea(a, minInd + 1, limit),
            )
        }
    }

    private fun findMinInArray(a: IntArray, start: Int, limit: Int): Int {
        var min = Int.MAX_VALUE
        var minIndex = -1
        for (index in start until limit) {
            if (a[index] < min) {
                min = a[index]
                minIndex = index
            }
        }
        return minIndex
    }

    private fun checkIfSorted(a: IntArray, start: Int, limit: Int): Boolean {
        for (i in start + 1 until limit) {
            if (a[i] < a[i - 1]) {
                return false
            }
        }
        return true
    }

    private fun maxOfThreeNums(a: Int, b: Int, c: Int): Int {
        return max(max(a, b), c)
    }
}
```