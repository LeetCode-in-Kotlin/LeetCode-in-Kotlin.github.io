[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3325\. Count Substrings With K-Frequency Characters I

Medium

Given a string `s` and an integer `k`, return the total number of substrings of `s` where **at least one** character appears **at least** `k` times.

**Example 1:**

**Input:** s = "abacb", k = 2

**Output:** 4

**Explanation:**

The valid substrings are:

*   `"aba"` (character `'a'` appears 2 times).
*   `"abac"` (character `'a'` appears 2 times).
*   `"abacb"` (character `'a'` appears 2 times).
*   `"bacb"` (character `'b'` appears 2 times).

**Example 2:**

**Input:** s = "abcde", k = 1

**Output:** 15

**Explanation:**

All substrings are valid because every character appears at least once.

**Constraints:**

*   `1 <= s.length <= 3000`
*   `1 <= k <= s.length`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numberOfSubstrings(s: String, k: Int): Int {
        var left = 0
        var result = 0
        val count = IntArray(26)
        for (i in 0 until s.length) {
            val ch = s[i]
            count[ch.code - 'a'.code]++

            while (count[ch.code - 'a'.code] == k) {
                result += s.length - i
                val atLeft = s[left]
                count[atLeft.code - 'a'.code]--
                left++
            }
        }
        return result
    }
}
```