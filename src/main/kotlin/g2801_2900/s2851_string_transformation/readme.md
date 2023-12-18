[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2851\. String Transformation

Hard

You are given two strings `s` and `t` of equal length `n`. You can perform the following operation on the string `s`:

*   Remove a **suffix** of `s` of length `l` where `0 < l < n` and append it at the start of `s`.   
    For example, let `s = 'abcd'` then in one operation you can remove the suffix `'cd'` and append it in front of `s` making `s = 'cdab'`.

You are also given an integer `k`. Return _the number of ways in which_ `s` _can be transformed into_ `t` _in **exactly**_ `k` _operations._

Since the answer can be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "abcd", t = "cdab", k = 2

**Output:** 2

**Explanation:** 

First way: 

In first operation, choose suffix from index = 3, so resulting s = "dabc". 

In second operation, choose suffix from index = 3, so resulting s = "cdab". 

Second way: 

In first operation, choose suffix from index = 1, so resulting s = "bcda".

In second operation, choose suffix from index = 1, so resulting s = "cdab".

**Example 2:**

**Input:** s = "ababab", t = "ababab", k = 1

**Output:** 2

**Explanation:** 

First way: 

Choose suffix from index = 2, so resulting s = "ababab". 

Second way: 

Choose suffix from index = 4, so resulting s = "ababab".

**Constraints:**

*   <code>2 <= s.length <= 5 * 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>15</sup></code>
*   `s.length == t.length`
*   `s` and `t` consist of only lowercase English alphabets.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var g: Array<LongArray>

    fun numberOfWays(s: String, t: String, k: Long): Int {
        val n = s.length
        val v = kmp(s + s, t)
        g = arrayOf(longArrayOf((v - 1).toLong(), v.toLong()), longArrayOf((n - v).toLong(), (n - 1 - v).toLong()))
        val f = qmi(k)
        return if (s == t) f[0][0].toInt() else f[0][1].toInt()
    }

    private fun kmp(s: String, p: String): Int {
        var s = s
        var p = p
        val n = p.length
        val m = s.length
        s = "#$s"
        p = "#$p"
        val ne = IntArray(n + 1)
        var j = 0
        for (i in 2..n) {
            while (j > 0 && p[i] != p[j + 1]) {
                j = ne[j]
            }
            if (p[i] == p[j + 1]) {
                j++
            }
            ne[i] = j
        }
        var cnt = 0
        j = 0
        for (i in 1..m) {
            while (j > 0 && s[i] != p[j + 1]) {
                j = ne[j]
            }
            if (s[i] == p[j + 1]) {
                j++
            }
            if (j == n) {
                if (i - n + 1 <= n) {
                    cnt++
                }
                j = ne[j]
            }
        }
        return cnt
    }

    private fun mul(c: Array<LongArray>, a: Array<LongArray>, b: Array<LongArray>) {
        val t = Array(2) { LongArray(2) }
        for (i in 0..1) {
            for (j in 0..1) {
                for (k in 0..1) {
                    val mod = 1e9.toInt() + 7
                    t[i][j] = (t[i][j] + a[i][k] * b[k][j]) % mod
                }
            }
        }
        for (i in 0..1) {
            System.arraycopy(t[i], 0, c[i], 0, 2)
        }
    }

    private fun qmi(k: Long): Array<LongArray> {
        var k = k
        val f = Array(2) { LongArray(2) }
        f[0][0] = 1
        while (k > 0) {
            if ((k and 1L) == 1L) {
                mul(f, f, g)
            }
            mul(g, g, g)
            k = k shr 1
        }
        return f
    }
}
```