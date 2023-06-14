[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1592\. Rearrange Spaces Between Words

Easy

You are given a string `text` of words that are placed among some number of spaces. Each word consists of one or more lowercase English letters and are separated by at least one space. It's guaranteed that `text` **contains at least one word**.

Rearrange the spaces so that there is an **equal** number of spaces between every pair of adjacent words and that number is **maximized**. If you cannot redistribute all the spaces equally, place the **extra spaces at the end**, meaning the returned string should be the same length as `text`.

Return _the string after rearranging the spaces_.

**Example 1:**

**Input:** text = " this is a sentence "

**Output:** "this is a sentence"

**Explanation:** There are a total of 9 spaces and 4 words. We can evenly divide the 9 spaces between the words: 9 / (4-1) = 3 spaces.

**Example 2:**

**Input:** text = " practice makes perfect"

**Output:** "practice makes perfect "

**Explanation:** There are a total of 7 spaces and 3 words. 7 / (3-1) = 3 spaces plus 1 extra space. We place this extra space at the end of the string.

**Constraints:**

*   `1 <= text.length <= 100`
*   `text` consists of lowercase English letters and `' '`.
*   `text` contains at least one word.

## Solution

```kotlin
class Solution {
    fun reorderSpaces(text: String): String {
        var spaceCount = 0
        for (c in text.toCharArray()) {
            if (c == ' ') {
                spaceCount++
            }
        }
        val words = text.trim { it <= ' ' }.split("\\s+".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        if (words.size == 1) {
            val sb = StringBuilder(words[0])
            for (i in 0 until spaceCount) {
                sb.append(" ")
            }
            return sb.toString()
        }
        val trailingSpaces = spaceCount % (words.size - 1)
        val newSpaces = spaceCount / (words.size - 1)
        val sb = StringBuilder()
        for (j in words.indices) {
            sb.append(words[j])
            if (j < words.size - 1) {
                for (i in 0 until newSpaces) {
                    sb.append(" ")
                }
            } else {
                for (i in 0 until trailingSpaces) {
                    sb.append(" ")
                }
            }
        }
        return sb.toString()
    }
}
```