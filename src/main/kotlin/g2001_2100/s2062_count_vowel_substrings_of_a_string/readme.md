[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2062\. Count Vowel Substrings of a String

Easy

A **substring** is a contiguous (non-empty) sequence of characters within a string.

A **vowel substring** is a substring that **only** consists of vowels (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) and has **all five** vowels present in it.

Given a string `word`, return _the number of **vowel substrings** in_ `word`.

**Example 1:**

**Input:** word = "aeiouu"

**Output:** 2

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "**aeiou**u" 

- "**aeiouu**"

**Example 2:**

**Input:** word = "unicornarihan"

**Output:** 0

**Explanation:** Not all 5 vowels are present, so there are no vowel substrings.

**Example 3:**

**Input:** word = "cuaieuouac"

**Output:** 7

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "c**uaieuo**uac" 

- "c**uaieuou**ac" 

- "c**uaieuoua**c" 

- "cu**aieuo**uac" 

- "cu**aieuou**ac" 

- "cu**aieuoua**c" 

- "cua**ieuoua**c"

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun countVowelSubstrings(word: String): Int {
        var count = 0
        val vowels: Set<Char> = HashSet(listOf('a', 'e', 'i', 'o', 'u'))
        val window: MutableSet<Char> = HashSet()
        for (i in word.indices) {
            window.clear()
            if (vowels.contains(word[i])) {
                window.add(word[i])
                for (j in i + 1 until word.length) {
                    if (!vowels.contains(word[j])) {
                        break
                    } else {
                        window.add(word[j])
                        if (window.size == 5) {
                            count++
                        }
                    }
                }
            }
        }
        return count
    }
}
```