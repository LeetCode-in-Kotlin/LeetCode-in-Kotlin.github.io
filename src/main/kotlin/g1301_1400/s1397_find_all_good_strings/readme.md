[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1397\. Find All Good Strings

Hard

Given the strings `s1` and `s2` of size `n` and the string `evil`, return _the number of **good** strings_.

A **good** string has size `n`, it is alphabetically greater than or equal to `s1`, it is alphabetically smaller than or equal to `s2`, and it does not contain the string `evil` as a substring. Since the answer can be a huge number, return this **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 2, s1 = "aa", s2 = "da", evil = "b"

**Output:** 51

**Explanation:** There are 25 good strings starting with 'a': "aa","ac","ad",...,"az". Then there are 25 good strings starting with 'c': "ca","cc","cd",...,"cz" and finally there is one good string starting with 'd': "da".

**Example 2:**

**Input:** n = 8, s1 = "leetcode", s2 = "leetgoes", evil = "leet"

**Output:** 0

**Explanation:** All strings greater than or equal to s1 and smaller than or equal to s2 start with the prefix "leet", therefore, there is not any good string.

**Example 3:**

**Input:** n = 2, s1 = "gx", s2 = "gz", evil = "x"

**Output:** 2

**Constraints:**

*   `s1.length == n`
*   `s2.length == n`
*   `s1 <= s2`
*   `1 <= n <= 500`
*   `1 <= evil.length <= 50`
*   All strings consist of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING", "UNUSED_PARAMETER")
class Solution {
    private val mod = 1000000007
    private lateinit var next: IntArray

    fun findGoodStrings(n: Int, s1: String, s2: String, evil: String): Int {
        var s1 = s1
        val s1arr = s1.toCharArray()
        for (i in s1.length - 1 downTo 0) {
            if (s1arr[i] > 'a') {
                s1arr[i] = (s1arr[i].code - 1).toChar()
                break
            } else {
                s1arr[i] = 'z'
            }
        }
        s1 = String(s1arr)
        next = getNext(evil)
        return if (s1.compareTo(s2) > 0) {
            lessOrEqualThan(s2, evil)
        } else (lessOrEqualThan(s2, evil) - lessOrEqualThan(s1, evil) + mod) % mod
    }

    private fun lessOrEqualThan(s: String, e: String): Int {
        val dp = Array(s.length + 1) { Array(e.length + 1) { LongArray(2) } }
        dp[0][0][1] = 1
        var res: Long = 0
        for (i in 0 until s.length) {
            for (state in 0 until e.length) {
                run {
                    var c = 'a'
                    while (c <= 'z') {
                        val nextstate = getNextState(state, c, e)
                        dp[i + 1][nextstate][0] = (dp[i + 1][nextstate][0] + dp[i][state][0]) % mod
                        c++
                    }
                }
                var c = 'a'
                while (c < s[i]) {
                    val nextstate = getNextState(state, c, e)
                    dp[i + 1][nextstate][0] = (dp[i + 1][nextstate][0] + dp[i][state][1]) % mod
                    c++
                }
                val nextstate = getNextState(state, s[i], e)
                dp[i + 1][nextstate][1] = (dp[i + 1][nextstate][1] + dp[i][state][1]) % mod
            }
        }
        for (i in 0 until e.length) {
            res = (res + dp[s.length][i][0]) % mod
            res = (res + dp[s.length][i][1]) % mod
        }
        return res.toInt()
    }

    private fun getNextState(prevState: Int, nextChar: Char, evil: String): Int {
        var idx = prevState
        while (idx != -1 && evil[idx] != nextChar) {
            idx = next[idx]
        }
        return idx + 1
    }

    private fun getNext(e: String): IntArray {
        val len = e.length
        val localNext = IntArray(len)
        localNext[0] = -1
        var last = -1
        var i = 0
        while (i < len - 1) {
            if (last == -1 || e[i] == e[last]) {
                i++
                last++
                localNext[i] = last
            } else {
                last = localNext[last]
            }
        }
        return localNext
    }
}
```