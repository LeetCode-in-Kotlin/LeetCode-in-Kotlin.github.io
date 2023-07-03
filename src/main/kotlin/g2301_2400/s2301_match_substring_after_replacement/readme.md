[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2301\. Match Substring After Replacement

Hard

You are given two strings `s` and `sub`. You are also given a 2D character array `mappings` where <code>mappings[i] = [old<sub>i</sub>, new<sub>i</sub>]</code> indicates that you may **replace** any number of <code>old<sub>i</sub></code> characters of `sub` with <code>new<sub>i</sub></code>. Each character in `sub` **cannot** be replaced more than once.

Return `true` _if it is possible to make_ `sub` _a substring of_ `s` _by replacing zero or more characters according to_ `mappings`. Otherwise, return `false`.

A **substring** is a contiguous non-empty sequence of characters within a string.

**Example 1:**

**Input:** s = "fool3e7bar", sub = "leet", mappings = \[\["e","3"],["t","7"],["t","8"]]

**Output:** true

**Explanation:** Replace the first 'e' in sub with '3' and 't' in sub with '7'.

Now sub = "l3e7" is a substring of s, so we return true.

**Example 2:**

**Input:** s = "fooleetbar", sub = "f00l", mappings = \[\["o","0"]]

**Output:** false

**Explanation:** The string "f00l" is not a substring of s and no replacements can be made.

Note that we cannot replace '0' with 'o'. 

**Example 3:**

**Input:** s = "Fool33tbaR", sub = "leetd", mappings = \[\["e","3"],["t","7"],["t","8"],["d","b"],["p","b"]]

**Output:** true

**Explanation:** Replace the first and second 'e' in sub with '3' and 'd' in sub with 'b'.

Now sub = "l33tb" is a substring of s, so we return true. 

**Constraints:**

*   `1 <= sub.length <= s.length <= 5000`
*   `0 <= mappings.length <= 1000`
*   `mappings[i].length == 2`
*   <code>old<sub>i</sub> != new<sub>i</sub></code>
*   `s` and `sub` consist of uppercase and lowercase English letters and digits.
*   <code>old<sub>i</sub></code> and <code>new<sub>i</sub></code> are either uppercase or lowercase English letters or digits.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var c1: CharArray
    private lateinit var c2: CharArray
    private lateinit var al: Array<MutableSet<Char>?>
    fun matchReplacement(s: String, sub: String, mappings: Array<CharArray>): Boolean {
        c1 = s.toCharArray()
        c2 = sub.toCharArray()
        al = arrayOfNulls(75)
        for (i in 0..74) {
            val temp: MutableSet<Char> = HashSet()
            al[i] = temp
        }
        for (mapping in mappings) {
            al[mapping[0].code - '0'.code]!!.add(mapping[1])
        }
        return ans(c1.size, c2.size) == 1
    }

    private fun ans(m: Int, n: Int): Int {
        var m = m
        var n = n
        if (m == 0) {
            return 0
        }
        if (ans(m - 1, n) == 1) {
            return 1
        }
        if (m >= n && (c1[m - 1] == c2[n - 1] || al[c2[n - 1].code - '0'.code]!!.contains(c1[m - 1]))) {
            while (n >= 1 && (c1[m - 1] == c2[n - 1] || al[c2[n - 1].code - '0'.code]!!.contains(c1[m - 1]))) {
                n--
                m--
            }
            if (n == 0) {
                return 1
            }
        }
        return 0
    }
}
```