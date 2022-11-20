[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 345\. Reverse Vowels of a String

Easy

Given a string `s`, reverse only all the vowels in the string and return it.

The vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`, and they can appear in both lower and upper cases, more than once.

**Example 1:**

**Input:** s = "hello"

**Output:** "holle"

**Example 2:**

**Input:** s = "leetcode"

**Output:** "leotcede"

**Constraints:**

*   <code>1 <= s.length <= 3 * 10<sup>5</sup></code>
*   `s` consist of **printable ASCII** characters.

## Solution

```kotlin
class Solution {
    private fun isVowel(c: Char): Boolean {
        return c == 'a' || c == 'A' || c == 'e' || c == 'E' || c == 'i' || c == 'I' || c == 'o' || c == 'O' ||
            c == 'u' || c == 'U'
    }

    fun reverseVowels(str: String): String {
        var i = 0
        var j = str.length - 1
        val str1 = str.toCharArray()
        while (i < j) {
            if (!isVowel(str1[i])) {
                i++
            } else if (!isVowel(str1[j])) {
                j--
            } else {
                // swapping
                val t = str1[i]
                str1[i] = str1[j]
                str1[j] = t
                i++
                j--
            }
        }
        return String(str1)
    }
}
```