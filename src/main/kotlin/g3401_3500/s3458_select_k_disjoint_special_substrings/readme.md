[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3458\. Select K Disjoint Special Substrings

Medium

Given a string `s` of length `n` and an integer `k`, determine whether it is possible to select `k` disjoint **special substrings**.

A **special substring** is a **substring** where:

*   Any character present inside the substring should not appear outside it in the string.
*   The substring is not the entire string `s`.

**Note** that all `k` substrings must be disjoint, meaning they cannot overlap.

Return `true` if it is possible to select `k` such disjoint special substrings; otherwise, return `false`.

**Example 1:**

**Input:** s = "abcdbaefab", k = 2

**Output:** true

**Explanation:**

*   We can select two disjoint special substrings: `"cd"` and `"ef"`.
*   `"cd"` contains the characters `'c'` and `'d'`, which do not appear elsewhere in `s`.
*   `"ef"` contains the characters `'e'` and `'f'`, which do not appear elsewhere in `s`.

**Example 2:**

**Input:** s = "cdefdc", k = 3

**Output:** false

**Explanation:**

There can be at most 2 disjoint special substrings: `"e"` and `"f"`. Since `k = 3`, the output is `false`.

**Example 3:**

**Input:** s = "abeabe", k = 0

**Output:** true

**Constraints:**

*   <code>2 <= n == s.length <= 5 * 10<sup>4</sup></code>
*   `0 <= k <= 26`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxSubstringLength(s: String, k: Int): Boolean {
        if (k == 0) return true
        val n = s.length
        val left = IntArray(26) { n }
        val right = IntArray(26) { -1 }
        for (i in 0 until n) {
            val c = s[i] - 'a'
            left[c] = minOf(left[c], i)
            right[c] = maxOf(right[c], i)
        }
        val intervals: MutableList<IntArray> = ArrayList()
        for (i in 0 until n) {
            if (i != left[s[i] - 'a']) {
                continue
            }
            var end = right[s[i] - 'a']
            var j = i
            var valid = true
            while (j <= end) {
                if (left[s[j] - 'a'] < i) {
                    valid = false
                    break
                }
                end =
                    maxOf(end, right[s[j] - 'a'])
                j++
            }
            if (valid && !(i == 0 && end == n - 1)) {
                intervals.add(intArrayOf(i, end))
            }
        }
        intervals.sortBy { it[1] }
        var count = 0
        var prev = -1
        for (inter in intervals) {
            if (inter[0] > prev) {
                count++
                prev = inter[1]
            }
        }
        return count >= k
    }
}
```