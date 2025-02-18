[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3453\. Separate Squares I

Medium

You are given a 2D integer array `squares`. Each <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code> represents the coordinates of the bottom-left point and the side length of a square parallel to the x-axis.

Find the **minimum** y-coordinate value of a horizontal line such that the total area of the squares above the line _equals_ the total area of the squares below the line.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Note**: Squares **may** overlap. Overlapping areas should be counted **multiple times**.

**Example 1:**

**Input:** squares = \[\[0,0,1],[2,2,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/06/4062example1drawio.png)

Any horizontal line between `y = 1` and `y = 2` will have 1 square unit above it and 1 square unit below it. The lowest option is 1.

**Example 2:**

**Input:** squares = \[\[0,0,2],[1,1,1]]

**Output:** 1.16667

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4062example2drawio.png)

The areas are:

*   Below the line: `7/6 * 2 (Red) + 1/6 (Blue) = 15/6 = 2.5`.
*   Above the line: `5/6 * 2 (Red) + 5/6 (Blue) = 15/6 = 2.5`.

Since the areas above and below the line are equal, the output is `7/6 = 1.16667`.

**Constraints:**

*   <code>1 <= squares.length <= 5 * 10<sup>4</sup></code>
*   <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code>
*   `squares[i].length == 3`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= l<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun separateSquares(squares: Array<IntArray>): Double {
        val n = squares.size
        val arr = Array(n) { LongArray(3) }
        var total = 0.0
        var left = Long.MAX_VALUE
        var right = Long.MIN_VALUE
        for (i in 0..n - 1) {
            val y = squares[i][1].toLong()
            val z = squares[i][2].toLong()
            arr[i][0] = y
            arr[i][1] = y + z
            arr[i][2] = z
            total += (z * z).toDouble()
            left = minOf(left, arr[i][0])
            right = maxOf(right, arr[i][1])
        }
        while (left < right) {
            val mid = (left + right) / 2
            var low = 0.0
            for (a in arr) {
                if (a[0] >= mid) {
                    continue
                } else if (a[1] <= mid) {
                    low += a[2] * a[2]
                } else {
                    low += a[2] * (mid - a[0])
                }
            }
            if (low + low + 0.00001 >= total) {
                right = mid
            } else {
                left = mid + 1
            }
        }
        left = right - 1
        var a1 = 0.0
        var a2 = 0.0
        for (a in arr) {
            val x = a[0]
            val y = a[1]
            val z = a[2]
            if (left > x) {
                a1 += (minOf(y, left) - x) * z.toDouble()
            }
            if (right < y) {
                a2 += (y - maxOf(x, right)) * z.toDouble()
            }
        }
        val goal = (total - a1 - a1) / 2
        val len = total - a1 - a2
        return right - 1 + (goal / len)
    }
}
```