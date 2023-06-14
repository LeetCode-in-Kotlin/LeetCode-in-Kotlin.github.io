[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1240\. Tiling a Rectangle with the Fewest Squares

Hard

Given a rectangle of size `n` x `m`, return _the minimum number of integer-sided squares that tile the rectangle_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/17/sample_11_1592.png)

**Input:** n = 2, m = 3

**Output:** 3

**Explanation:** `3` squares are necessary to cover the rectangle. 

`2` (squares of `1x1`) 

`1` (square of `2x2`)

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/10/17/sample_22_1592.png)

**Input:** n = 5, m = 8

**Output:** 5

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/10/17/sample_33_1592.png)

**Input:** n = 11, m = 13

**Output:** 6

**Constraints:**

*   `1 <= n, m <= 13`

## Solution

```kotlin
class Solution {
    private var n = 0
    private var m = 0
    private lateinit var covered: Array<BooleanArray>
    private var res = 0

    fun tilingRectangle(n: Int, m: Int): Int {
        this.n = n
        this.m = m
        covered = Array(n) { BooleanArray(m) }
        res = m * n
        backtrack(0)
        return res
    }

    private fun backtrack(count: Int) {
        if (count >= res) {
            return
        }
        var find = false
        for (r in 0 until n) {
            for (c in 0 until m) {
                if (!covered[r][c]) {
                    find = true
                    var len = findMaxWidth(r, c)
                    while (len > 0) {
                        cover(r, c, len, true)
                        backtrack(count + 1)
                        cover(r, c, len, false)
                        len--
                    }
                    break
                }
            }
            if (find) {
                break
            }
        }
        if (!find) {
            res = count
        }
    }

    private fun cover(r: Int, c: Int, len: Int, flag: Boolean) {
        for (i in r until r + len) {
            for (j in c until c + len) {
                covered[i][j] = flag
            }
        }
    }

    private fun findMaxWidth(r: Int, c: Int): Int {
        var len = Math.min(n - r, m - c)
        while (true) {
            var find = false
            for (i in r until r + len) {
                for (j in c until c + len) {
                    if (covered[i][j]) {
                        find = true
                        len = Math.min(i - r, j - c)
                        break
                    }
                }
                if (find) {
                    break
                }
            }
            if (!find) {
                break
            }
        }
        return len
    }
}
```