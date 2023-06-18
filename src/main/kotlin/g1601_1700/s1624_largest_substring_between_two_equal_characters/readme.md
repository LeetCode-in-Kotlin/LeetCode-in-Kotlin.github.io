[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1624\. Largest Substring Between Two Equal Characters

Easy

Given a string `s`, return _the length of the longest substring between two equal characters, excluding the two characters._ If there is no such substring return `-1`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "aa"

**Output:** 0

**Explanation:** The optimal substring here is an empty substring between the two `'a's`.

**Example 2:**

**Input:** s = "abca"

**Output:** 2

**Explanation:** The optimal substring here is "bc".

**Example 3:**

**Input:** s = "cbzxy"

**Output:** -1

**Explanation:** There are no characters that appear twice in s.

**Constraints:**

*   `1 <= s.length <= 300`
*   `s` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxLengthBetweenEqualCharacters(s: String): Int {
        val lenS = s.length
        val firstIndexesOf = IntArray(26) { -1 }
        var longest = -1
        for (idx in 0 until lenS) {
            val idxCh = (s[idx] - 'a')
            if (firstIndexesOf[idxCh] < 0) {
                firstIndexesOf[idxCh] = idx
            } else {
                val len = idx - firstIndexesOf[idxCh] + 1 - 2
                longest = maxOf(longest, len)
            }
        }
        return longest
    }
}
```