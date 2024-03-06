[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3006\. Find Beautiful Indices in the Given Array I

Medium

You are given a **0-indexed** string `s`, a string `a`, a string `b`, and an integer `k`.

An index `i` is **beautiful** if:

*   `0 <= i <= s.length - a.length`
*   `s[i..(i + a.length - 1)] == a`
*   There exists an index `j` such that:
    *   `0 <= j <= s.length - b.length`
    *   `s[j..(j + b.length - 1)] == b`
    *   `|j - i| <= k`

Return _the array that contains beautiful indices in **sorted order from smallest to largest**_.

**Example 1:**

**Input:** s = "isawsquirrelnearmysquirrelhouseohmy", a = "my", b = "squirrel", k = 15

**Output:** [16,33]

**Explanation:** There are 2 beautiful indices: [16,33]. 
- The index 16 is beautiful as s[16..17] == "my" and there exists an index 4 with s[4..11] == "squirrel" and \|16 - 4\| <= 15. 
- The index 33 is beautiful as s[33..34] == "my" and there exists an index 18 with s[18..25] == "squirrel" and \|33 - 18\| <= 15. Thus we return [16,33] as the result.

**Example 2:**

**Input:** s = "abcd", a = "a", b = "a", k = 4

**Output:** [0]

**Explanation:** There is 1 beautiful index: [0]. 
- The index 0 is beautiful as s[0..0] == "a" and there exists an index 0 with s[0..0] == "a" and \|0 - 0\| <= 4. Thus we return [0] as the result.

**Constraints:**

*   <code>1 <= k <= s.length <= 10<sup>5</sup></code>
*   `1 <= a.length, b.length <= 10`
*   `s`, `a`, and `b` contain only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun beautifulIndices(s: String, a: String, b: String, q: Int): List<Int> {
        val sc = s.toCharArray()
        val ac = a.toCharArray()
        val bc = b.toCharArray()
        val lpsa = getLps(ac)
        val lpsb = getLps(bc)
        val comp = IntArray(sc.size)
        val st = IntArray(sc.size)
        var si = 0
        var k: Int
        var mo = -bc.size + 1
        if (bc[0] == sc[0]) {
            comp[0] = 1
            if (bc.size == 1) {
                st[si++] = mo
            }
        }
        for (i in 1 until comp.size) {
            mo++
            if (sc[i] == bc[0]) {
                comp[i] = 1
            }
            k = comp[i - 1]
            if (k == bc.size) {
                k = lpsb[k - 1]
            }
            while (k > 0) {
                if (bc[k] == sc[i]) {
                    comp[i] = k + 1
                    break
                }
                k = lpsb[k - 1]
            }
            if (comp[i] == bc.size) {
                st[si++] = mo
            }
        }
        var sia = 0
        mo = -ac.size + 1
        val ret: MutableList<Int> = ArrayList()
        if (si == 0) {
            return ret
        }
        if (sc[0] == ac[0]) {
            comp[0] = 1
            if (ac.size == 1 && st[0] <= q) {
                ret.add(0)
            }
        } else {
            comp[0] = 0
        }
        for (i in 1 until comp.size) {
            mo++
            if (sc[i] == ac[0]) {
                comp[i] = 1
            } else {
                comp[i] = 0
            }
            k = comp[i - 1]
            if (k == ac.size) {
                k = lpsa[k - 1]
            }
            while (k > 0) {
                if (ac[k] == sc[i]) {
                    comp[i] = k + 1
                    break
                }
                k = lpsa[k - 1]
            }
            if (comp[i] == ac.size) {
                while (sia < si && st[sia] + q < mo) {
                    sia++
                }
                if (sia == si) {
                    break
                }
                if (mo >= st[sia] - q && mo <= st[sia] + q) {
                    ret.add(mo)
                }
            }
        }
        return ret
    }

    private fun getLps(xc: CharArray): IntArray {
        val r = IntArray(xc.size)
        var k: Int
        for (i in 1 until xc.size) {
            if (xc[i] == xc[0]) {
                r[i] = 1
            }
            k = r[i - 1]
            while (k > 0) {
                if (xc[k] == xc[i]) {
                    r[i] = k + 1
                    break
                }
                k = r[k - 1]
            }
        }
        return r
    }
}
```