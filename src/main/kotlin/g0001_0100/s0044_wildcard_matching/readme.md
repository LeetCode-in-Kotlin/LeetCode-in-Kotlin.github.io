[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 44\. Wildcard Matching

Hard

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

*   `'?'` Matches any single character.
*   `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

**Example 1:**

**Input:** s = "aa", p = "a"

**Output:** false

**Explanation:** "a" does not match the entire string "aa".

**Example 2:**

**Input:** s = "aa", p = "\*"

**Output:** true

**Explanation:** '\*' matches any sequence.

**Example 3:**

**Input:** s = "cb", p = "?a"

**Output:** false

**Explanation:** '?' matches 'c', but the second letter is 'a', which does not match 'b'.

**Constraints:**

*   `0 <= s.length, p.length <= 2000`
*   `s` contains only lowercase English letters.
*   `p` contains only lowercase English letters, `'?'` or `'*'`.

## Solution

```kotlin
class Solution {
    fun isMatch(inputString: String, pattern: String): Boolean {
        var i = 0
        var j = 0
        var starIdx = -1
        var lastMatch = -1
        while (i < inputString.length) {
            if (j < pattern.length &&
                (inputString[i] == pattern[j] || pattern[j] == '?')
            ) {
                i++
                j++
            } else if (j < pattern.length && pattern[j] == '*') {
                starIdx = j
                lastMatch = i
                j++
            } else if (starIdx != -1) {
                // there is a no match and there was a previous star, we will reset the j to indx
                // after star_index
                // lastMatch will tell from which index we start comparing the string if we
                // encounter * in pattern
                j = starIdx + 1
                // we are saying we included more characters in * so we incremented the
                lastMatch++
                // index
                i = lastMatch
            } else {
                return false
            }
        }
        var isMatch = true
        while (j < pattern.length && pattern[j] == '*') {
            j++
        }
        if (i != inputString.length || j != pattern.length) {
            isMatch = false
        }
        return isMatch
    }
}
```