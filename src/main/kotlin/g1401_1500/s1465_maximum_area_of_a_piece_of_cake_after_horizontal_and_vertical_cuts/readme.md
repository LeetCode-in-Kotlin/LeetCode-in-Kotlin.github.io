[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1465\. Maximum Area of a Piece of Cake After Horizontal and Vertical Cuts

Medium

You are given a rectangular cake of size `h x w` and two arrays of integers `horizontalCuts` and `verticalCuts` where:

*   `horizontalCuts[i]` is the distance from the top of the rectangular cake to the <code>i<sup>th</sup></code> horizontal cut and similarly, and
*   `verticalCuts[j]` is the distance from the left of the rectangular cake to the <code>j<sup>th</sup></code> vertical cut.

Return _the maximum area of a piece of cake after you cut at each horizontal and vertical position provided in the arrays_ `horizontalCuts` _and_ `verticalCuts`. Since the answer can be a large number, return this **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/05/14/leetcode_max_area_2.png)

**Input:** h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]

**Output:** 4

**Explanation:** The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green piece of cake has the maximum area.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/05/14/leetcode_max_area_3.png)

**Input:** h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]

**Output:** 6

**Explanation:** The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green and yellow pieces of cake have the maximum area.

**Example 3:**

**Input:** h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]

**Output:** 9

**Constraints:**

*   <code>2 <= h, w <= 10<sup>9</sup></code>
*   <code>1 <= horizontalCuts.length <= min(h - 1, 10<sup>5</sup>)</code>
*   <code>1 <= verticalCuts.length <= min(w - 1, 10<sup>5</sup>)</code>
*   `1 <= horizontalCuts[i] < h`
*   `1 <= verticalCuts[i] < w`
*   All the elements in `horizontalCuts` are distinct.
*   All the elements in `verticalCuts` are distinct.

## Solution

```kotlin
class Solution {
    fun maxArea(h: Int, w: Int, horizontalCuts: IntArray, verticalCuts: IntArray): Int {
        horizontalCuts.sort()
        verticalCuts.sort()
        var maxVertical = Math.max(0L, verticalCuts[0].toLong())
        for (i in 1 until verticalCuts.size) {
            val diff = verticalCuts[i] - verticalCuts[i - 1]
            maxVertical = Math.max(maxVertical, diff.toLong())
        }
        maxVertical = Math.max(maxVertical, w.toLong() - verticalCuts[verticalCuts.size - 1])
        var maxHorizontal = Math.max(0L, horizontalCuts[0].toLong())
        for (i in 1 until horizontalCuts.size) {
            val diff = horizontalCuts[i] - horizontalCuts[i - 1]
            maxHorizontal = Math.max(maxHorizontal, diff.toLong())
        }
        maxHorizontal = Math.max(maxHorizontal, h.toLong() - horizontalCuts[horizontalCuts.size - 1])
        return (maxVertical % 1000000007 * maxHorizontal % 1000000007).toInt() % 1000000007
    }
}
```