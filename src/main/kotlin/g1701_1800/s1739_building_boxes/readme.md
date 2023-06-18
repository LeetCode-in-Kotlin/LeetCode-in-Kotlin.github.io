[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1739\. Building Boxes

Hard

You have a cubic storeroom where the width, length, and height of the room are all equal to `n` units. You are asked to place `n` boxes in this room where each box is a cube of unit side length. There are however some rules to placing the boxes:

*   You can place the boxes anywhere on the floor.
*   If box `x` is placed on top of the box `y`, then each side of the four vertical sides of the box `y` **must** either be adjacent to another box or to a wall.

Given an integer `n`, return _the **minimum** possible number of boxes touching the floor._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/3-boxes.png)

**Input:** n = 3

**Output:** 3

**Explanation:** The figure above is for the placement of the three boxes. These boxes are placed in the corner of the room, where the corner is on the left side.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/4-boxes.png)

**Input:** n = 4

**Output:** 3

**Explanation:** The figure above is for the placement of the four boxes. These boxes are placed in the corner of the room, where the corner is on the left side.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/04/10-boxes.png)

**Input:** n = 10

**Output:** 6

**Explanation:** The figure above is for the placement of the ten boxes. These boxes are placed in the corner of the room, where the corner is on the back side.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumBoxes(n: Int): Int {
        val k: Int = findLargestTetrahedralNotGreaterThan(n)
        val used: Int = tetrahedral(k)
        val floor: Int = triangular(k)
        val unused: Int = (n - used)
        if (unused == 0) {
            return floor
        }
        val r: Int = findSmallestTriangularNotLessThan(unused)
        return (floor + r)
    }

    private fun findLargestTetrahedralNotGreaterThan(te: Int): Int {
        var a: Int = Math.ceil(Math.pow(product(6, te.toLong()).toDouble(), ONE_THIRD)).toInt()
        while (tetrahedral(a) > te) {
            a--
        }
        return a
    }

    private fun findSmallestTriangularNotLessThan(t: Int): Int {
        var a: Int = -1 + Math.floor(Math.sqrt(product(t.toLong(), 2).toDouble())).toInt()
        while (triangular(a) < t) {
            a++
        }
        return a
    }

    private fun tetrahedral(a: Int): Int {
        return ratio(product(a.toLong(), (a + 1).toLong(), (a + 2).toLong()), 6).toInt()
    }

    private fun triangular(a: Int): Int {
        return ratio(product(a.toLong(), (a + 1).toLong()), 2).toInt()
    }

    private fun product(vararg vals: Long): Long {
        var product: Long = 1L
        for (`val`: Long in vals) {
            product *= `val`
        }
        return product
    }

    private fun ratio(a: Long, b: Long): Long {
        return (a / b)
    }

    companion object {
        val ONE_THIRD: Double = 1.0 / 3.0
    }
}
```