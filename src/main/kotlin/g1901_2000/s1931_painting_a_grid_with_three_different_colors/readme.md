[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1931\. Painting a Grid With Three Different Colors

Hard

You are given two integers `m` and `n`. Consider an `m x n` grid where each cell is initially white. You can paint each cell **red**, **green**, or **blue**. All cells **must** be painted.

Return _the number of ways to color the grid with **no two adjacent cells having the same color**_. Since the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)

**Input:** m = 1, n = 1

**Output:** 3

**Explanation:** The three possible colorings are shown in the image above.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)

**Input:** m = 1, n = 2

**Output:** 6

**Explanation:** The six possible colorings are shown in the image above.

**Example 3:**

**Input:** m = 5, n = 5

**Output:** 580986

**Constraints:**

*   `1 <= m <= 5`
*   `1 <= n <= 1000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun colorTheGrid(m: Int, n: Int): Int {
        if (m == 1) {
            return (3L * powMod(2, n - 1) % P).toInt()
        }
        if (m == 2) {
            return (6L * powMod(3, n - 1) % P).toInt()
        }
        if (n == 1) {
            return (3L * powMod(2, m - 1) % P).toInt()
        }
        if (n == 2) {
            return (6L * powMod(3, m - 1) % P).toInt()
        }
        val totalTemplates = 1 shl m - 2
        val totalPaintings = binPow(3, m)
        val paintingToTemplate = IntArray(totalPaintings)
        val paintingCountForTemplate = LongArray(totalTemplates)
        val templateEdgeCount = Array(totalTemplates) { LongArray(totalTemplates) }
        val templateToIndex: MutableMap<Int, Int> = HashMap(1 shl m - 2)
        val templateCounter = 0
        extracted(
            m,
            totalPaintings,
            paintingToTemplate,
            paintingCountForTemplate,
            templateToIndex,
            templateCounter
        )
        extracted(m, totalPaintings, paintingToTemplate, templateEdgeCount)
        for (i in 0 until totalTemplates) {
            val c = paintingCountForTemplate[i]
            for (j in 0 until totalTemplates) {
                templateEdgeCount[i][j] /= c
            }
        }
        val matrixPower = matrixPower(templateEdgeCount, n.toLong() - 1)
        var ans: Long = 0
        for (i in 0 until totalTemplates) {
            var s: Long = 0
            val arr = matrixPower[i]
            for (a in arr) {
                s += a
            }
            ans += paintingCountForTemplate[i] * s
        }
        return (ans % P).toInt()
    }

    private fun extracted(
        m: Int,
        totalPaintings: Int,
        paintingToTemplate: IntArray,
        templateEdgeCount: Array<LongArray>
    ) {
        for (i in 0 until totalPaintings) {
            if (paintingToTemplate[i] == -1) {
                continue
            }
            for (j in i + 1 until totalPaintings) {
                if (paintingToTemplate[j] == -1) {
                    continue
                }
                if (checkAllowance(i, j, m)) {
                    templateEdgeCount[paintingToTemplate[i]][paintingToTemplate[j]]++
                    templateEdgeCount[paintingToTemplate[j]][paintingToTemplate[i]]++
                }
            }
        }
    }

    private fun extracted(
        m: Int,
        totalPaintings: Int,
        paintingToTemplate: IntArray,
        paintingCountForTemplate: LongArray,
        templateToIndex: MutableMap<Int, Int>,
        templateCounter: Int
    ) {
        var templateCounter = templateCounter
        for (i in 0 until totalPaintings) {
            val type = getType(i, m)
            if (type == -1) {
                paintingToTemplate[i] = -1
                continue
            }
            var templateIndex = templateToIndex[type]
            if (templateIndex == null) {
                templateToIndex[type] = templateCounter
                templateIndex = templateCounter++
            }
            paintingToTemplate[i] = templateIndex
            paintingCountForTemplate[templateIndex]++
        }
    }

    private fun checkAllowance(a: Int, b: Int, m: Int): Boolean {
        var a = a
        var b = b
        for (i in 0 until m) {
            if (a % 3 == b % 3) {
                return false
            }
            a /= 3
            b /= 3
        }
        return true
    }

    private fun getType(a: Int, m: Int): Int {
        var a = a
        var m = m
        val digits = IntArray(3)
        val first = a % 3
        val second = a % 9 / 3
        if (first == second) {
            return -1
        }
        digits[second] = 1
        digits[3 - first - second] = 2
        var prev = second
        var type = 1
        m -= 2
        a /= 9
        while (m-- > 0) {
            val curr = a % 3
            if (prev == curr) {
                return -1
            }
            type = type * 3 + digits[curr]
            prev = curr
            a /= 3
        }
        return type
    }

    private fun powMod(a: Int, b: Int): Int {
        var a = a
        var b = b
        var res: Long = 1
        while (b != 0) {
            if (b and 1 != 0) {
                res = res * a % P
                --b
            } else {
                a = (a.toLong() * a % P).toInt()
                b = b shr 1
            }
        }
        return res.toInt()
    }

    private fun binPow(a: Int, n: Int): Int {
        var n = n
        var res = 1
        var tmp = a
        while (n != 0) {
            if (n and 1 != 0) {
                res *= tmp
            }
            tmp *= tmp
            n = n shr 1
        }
        return res
    }

    private fun matrixPower(base: Array<LongArray>, pow: Long): Array<LongArray> {
        var base = base
        var pow = pow
        val n = base.size
        var res = Array(n) { LongArray(n) }
        for (i in 0 until n) {
            res[i][i] = 1
        }
        while (pow != 0L) {
            if (pow and 1L != 0L) {
                res = multiplyMatrix(res, base)
                --pow
            } else {
                base = multiplyMatrix(base, base)
                pow = pow shr 1
            }
        }
        return res
    }

    private fun multiplyMatrix(a: Array<LongArray>, b: Array<LongArray>): Array<LongArray> {
        val n = a.size
        val ans = Array(n) { LongArray(n) }
        for (i in 0 until n) {
            for (j in 0 until n) {
                for (k in 0 until n) {
                    ans[i][j] += a[i][k] * b[k][j]
                }
                ans[i][j] %= P.toLong()
            }
        }
        return ans
    }

    companion object {
        const val P = 1000000007
    }
}
```