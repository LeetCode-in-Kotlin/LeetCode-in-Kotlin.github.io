[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1461\. Check If a String Contains All Binary Codes of Size K

Medium

Given a binary string `s` and an integer `k`, return `true` _if every binary code of length_ `k` _is a substring of_ `s`. Otherwise, return `false`.

**Example 1:**

**Input:** s = "00110110", k = 2

**Output:** true

**Explanation:** The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indices 0, 1, 3 and 2 respectively.

**Example 2:**

**Input:** s = "0110", k = 1

**Output:** true

**Explanation:** The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring.

**Example 3:**

**Input:** s = "0110", k = 2

**Output:** false

**Explanation:** The binary code "00" is of length 2 and does not exist in the array.

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.
*   `1 <= k <= 20`

## Solution

```kotlin
class Solution {
    fun hasAllCodes(s: String, k: Int): Boolean {
        val total = Math.pow(2.0, k.toDouble()).toInt()
        var start = 0
        var end = start + k
        val st: MutableSet<String> = HashSet()
        while (end <= s.length) {
            val sbStr = s.substring(start, end)
            st.add(sbStr)
            if (st.size == total) {
                return true
            }
            start++
            end++
        }
        return false
    }
}
```