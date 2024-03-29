[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2255\. Count Prefixes of a Given String

Easy

You are given a string array `words` and a string `s`, where `words[i]` and `s` comprise only of **lowercase English letters**.

Return _the **number of strings** in_ `words` _that are a **prefix** of_ `s`.

A **prefix** of a string is a substring that occurs at the beginning of the string. A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** words = ["a","b","c","ab","bc","abc"], s = "abc"

**Output:** 3

**Explanation:**

The strings in words which are a prefix of s = "abc" are:

"a", "ab", and "abc".

Thus the number of strings in words which are a prefix of s is 3.

**Example 2:**

**Input:** words = ["a","a"], s = "aa"

**Output:** 2

**Explanation:**

Both of the strings are a prefix of s.

Note that the same string can occur multiple times in words, and it should be counted each time.

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length, s.length <= 10`
*   `words[i]` and `s` consist of lowercase English letters **only**.

## Solution

```kotlin
class Solution {
    fun countPrefixes(words: Array<String>, s: String): Int {
        var count = 0
        for (str in words) {
            if (s.indexOf(str) == 0) {
                ++count
            }
        }
        return count
    }
}
```