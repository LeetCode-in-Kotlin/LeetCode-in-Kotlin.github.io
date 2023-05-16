[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1002\. Find Common Characters

Easy

Given a string array `words`, return _an array of all characters that show up in all strings within the_ `words` _(including duplicates)_. You may return the answer in **any order**.

**Example 1:**

**Input:** words = ["bella","label","roller"]

**Output:** ["e","l","l"]

**Example 2:**

**Input:** words = ["cool","lock","cook"]

**Output:** ["c","o"]

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun commonChars(words: Array<String>?): List<String> {
        if (words == null) {
            throw RuntimeException("words null")
        }
        if (words.isEmpty()) {
            return ArrayList()
        }
        var tmp = words[0]
        for (i in 1 until words.size) {
            tmp = getCommon(tmp, words[i])
        }
        val result: MutableList<String> = ArrayList()
        for (element in tmp) {
            result.add(element.toString())
        }
        return result
    }

    private fun getCommon(s1: String, s2: String): String {
        if (s1.isEmpty() || s2.isEmpty()) {
            return ""
        }
        val c1c = countChars(s1)
        val c2c = countChars(s2)
        val sb = StringBuilder()
        for (i in c1c.indices) {
            var m = c1c[i].coerceAtMost(c2c[i])
            while (m > 0) {
                sb.append(('a'.code + i).toChar())
                m--
            }
        }
        return sb.toString()
    }

    private fun countChars(str: String): IntArray {
        val result = IntArray(26)
        for (element in str) {
            result[element.code - 'a'.code]++
        }
        return result
    }
}
```