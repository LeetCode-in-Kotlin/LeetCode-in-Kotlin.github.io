[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2001\. Number of Pairs of Interchangeable Rectangles

Medium

You are given `n` rectangles represented by a **0-indexed** 2D integer array `rectangles`, where <code>rectangles[i] = [width<sub>i</sub>, height<sub>i</sub>]</code> denotes the width and height of the <code>i<sup>th</sup></code> rectangle.

Two rectangles `i` and `j` (`i < j`) are considered **interchangeable** if they have the **same** width-to-height ratio. More formally, two rectangles are **interchangeable** if <code>width<sub>i</sub>/height<sub>i</sub> == width<sub>j</sub>/height<sub>j</sub></code> (using decimal division, not integer division).

Return _the **number** of pairs of **interchangeable** rectangles in_ `rectangles`.

**Example 1:**

**Input:** rectangles = \[\[4,8],[3,6],[10,20],[15,30]]

**Output:** 6

**Explanation:** The following are the interchangeable pairs of rectangles by index (0-indexed): 

- Rectangle 0 with rectangle 1: 4/8 == 3/6. 

- Rectangle 0 with rectangle 2: 4/8 == 10/20. 

- Rectangle 0 with rectangle 3: 4/8 == 15/30. 

- Rectangle 1 with rectangle 2: 3/6 == 10/20. 

- Rectangle 1 with rectangle 3: 3/6 == 15/30. 

- Rectangle 2 with rectangle 3: 10/20 == 15/30.

**Example 2:**

**Input:** rectangles = \[\[4,5],[7,8]]

**Output:** 0

**Explanation:** There are no interchangeable pairs of rectangles.

**Constraints:**

*   `n == rectangles.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `rectangles[i].length == 2`
*   <code>1 <= width<sub>i</sub>, height<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun factorial(n: Long): Long {
        var n = n
        var m: Long = 0
        while (n > 0) {
            m += n
            n -= 1
        }
        return m
    }

    fun interchangeableRectangles(rec: Array<IntArray>): Long {
        val ratio = DoubleArray(rec.size)
        for (i in rec.indices) {
            ratio[i] = rec[i][0].toDouble() / rec[i][1]
        }
        ratio.sort()
        var res: Long = 0
        var k = 0
        for (j in 0 until ratio.size - 1) {
            if (ratio[j] == ratio[j + 1]) {
                k++
            }
            if (ratio[j] != ratio[j + 1] || j + 2 == ratio.size) {
                res += factorial(k.toLong())
                k = 0
            }
        }
        return res
    }
}
```