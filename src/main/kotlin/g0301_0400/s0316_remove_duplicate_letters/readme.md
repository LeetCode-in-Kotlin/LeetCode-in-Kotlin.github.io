[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 316\. Remove Duplicate Letters

Medium

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

**Example 1:**

**Input:** s = "bcabc"

**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"

**Output:** "acdb"

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

## Solution

```kotlin
class Solution {
    fun removeDuplicateLetters(s: String): String {
        val charCount = IntArray(26)
        val charAdded = BooleanArray(26)
        // Build the char count array
        for (c in s.toCharArray()) {
            charCount[c.code - 'a'.code] += 1
        }
        val sb = StringBuilder()
        // i = index of the input string
        // j = index of the output stringBuilder
        var j = 0
        for (i in 0 until s.length) {
            val curr = s[i]
            // If the curr char is NOT already added in the final string
            if (!charAdded[curr.code - 'a'.code]) {
                // If the prev char in final string is lexicographically greater than curr char of
                // input string
                // And there are more characters in charCount array then we can remove this prev
                // char from final string
                // Do this check iteratively until all characters are removed from the final string
                // or prev char < curr char
                while (j > 0 && sb[j - 1] > curr && charCount[sb[j - 1].code - 'a'.code] > 0) {
                    charAdded[sb[j - 1].code - 'a'.code] = false
                    sb.deleteCharAt(j - 1)
                    j--
                }
                // Add the curr char in final string and mark that character as added in the string
                sb.append(curr)
                charAdded[curr.code - 'a'.code] = true
                j++
            }
            // Reduce the count of the current character from the charCount array
            charCount[curr.code - 'a'.code] -= 1
        }
        return sb.toString()
    }
}
```