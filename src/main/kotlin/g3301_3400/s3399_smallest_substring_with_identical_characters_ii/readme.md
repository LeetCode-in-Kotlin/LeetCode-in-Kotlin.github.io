[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3399\. Smallest Substring With Identical Characters II

Hard

You are given a binary string `s` of length `n` and an integer `numOps`.

You are allowed to perform the following operation on `s` **at most** `numOps` times:

*   Select any index `i` (where `0 <= i < n`) and **flip** `s[i]`. If `s[i] == '1'`, change `s[i]` to `'0'` and vice versa.

You need to **minimize** the length of the **longest** substring of `s` such that all the characters in the substring are **identical**.

Return the **minimum** length after the operations.

**Example 1:**

**Input:** s = "000001", numOps = 1

**Output:** 2

**Explanation:**

By changing `s[2]` to `'1'`, `s` becomes `"001001"`. The longest substrings with identical characters are `s[0..1]` and `s[3..4]`.

**Example 2:**

**Input:** s = "0000", numOps = 2

**Output:** 1

**Explanation:**

By changing `s[0]` and `s[2]` to `'1'`, `s` becomes `"1010"`.

**Example 3:**

**Input:** s = "0101", numOps = 0

**Output:** 1

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   `s` consists only of `'0'` and `'1'`.
*   `0 <= numOps <= n`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minLength(s: String, numOps: Int): Int {
        val b = s.toByteArray()
        var flips1 = 0
        var flips2 = 0
        for (i in b.indices) {
            val e1 = (if (i % 2 == 0) '0' else '1').code.toByte()
            val e2 = (if (i % 2 == 0) '1' else '0').code.toByte()
            if (b[i] != e1) {
                flips1++
            }
            if (b[i] != e2) {
                flips2++
            }
        }
        val flips = min(flips1, flips2)
        if (flips <= numOps) {
            return 1
        }
        val seg: MutableList<Int> = ArrayList()
        var count = 1
        var max = 1
        for (i in 1..<b.size) {
            if (b[i] != b[i - 1]) {
                if (count != 1) {
                    seg.add(count)
                    max = max(max, count)
                }
                count = 1
            } else {
                count++
            }
        }
        if (count != 1) {
            seg.add(count)
            max = max(max, count)
        }
        var l = 2
        var r = max
        var res = max
        while (l <= r) {
            val m = l + (r - l) / 2
            if (check(m, seg, numOps)) {
                r = m - 1
                res = m
            } else {
                l = m + 1
            }
        }
        return res
    }

    private fun check(sz: Int, seg: MutableList<Int>, ops: Int): Boolean {
        var ops = ops
        for (i in seg) {
            val x = i / (sz + 1)
            ops -= x
            if (ops < 0) {
                return false
            }
        }
        return true
    }
}
```