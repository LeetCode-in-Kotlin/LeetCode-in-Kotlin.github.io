[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1044\. Longest Duplicate Substring

Hard

Given a string `s`, consider all _duplicated substrings_: (contiguous) substrings of s that occur 2 or more times. The occurrences may overlap.

Return **any** duplicated substring that has the longest possible length. If `s` does not have a duplicated substring, the answer is `""`.

**Example 1:**

**Input:** s = "banana"

**Output:** "ana"

**Example 2:**

**Input:** s = "abcd"

**Output:** ""

**Constraints:**

*   <code>2 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    private lateinit var hsh: LongArray
    private lateinit var pw: LongArray
    private val cnt: Array<MutableList<Int>> = Array(26) { ArrayList() }

    fun longestDupSubstring(s: String): String {
        val n = s.length
        val base = 131
        for (i in 0..25) {
            cnt[i] = ArrayList()
        }
        hsh = LongArray(n + 1)
        pw = LongArray(n + 1)
        pw[0] = 1
        for (j in 1..n) {
            hsh[j] = (hsh[j - 1] * base + s[j - 1].code.toLong()) % MOD
            pw[j] = pw[j - 1] * base % MOD
            cnt[s[j - 1].code - 'a'.code].add(j - 1)
        }
        var ans = ""
        for (i in 0..25) {
            if (cnt[i].isEmpty()) {
                continue
            }
            val idx: MutableList<Int> = cnt[i]
            var set: MutableSet<Long>
            var lo = 1
            var hi = n - idx[0]
            while (lo <= hi) {
                val len = (lo + hi) / 2
                set = HashSet()
                var found = false
                for (nxt in idx) {
                    if (nxt + len <= n) {
                        val substrHash = getSubstrHash(nxt, nxt + len)
                        if (set.contains(substrHash)) {
                            found = true
                            if (len + 1 > ans.length) {
                                ans = s.substring(nxt, nxt + len)
                            }
                            break
                        }
                        set.add(substrHash)
                    }
                }
                if (found) {
                    lo = len + 1
                } else {
                    hi = len - 1
                }
            }
        }
        return ans
    }

    private fun getSubstrHash(l: Int, r: Int): Long {
        return (hsh[r] - hsh[l] * pw[r - l] % MOD + MOD) % MOD
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```