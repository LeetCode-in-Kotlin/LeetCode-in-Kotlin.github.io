[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1961\. Check If String Is a Prefix of Array

Easy

Given a string `s` and an array of strings `words`, determine whether `s` is a **prefix string** of `words`.

A string `s` is a **prefix string** of `words` if `s` can be made by concatenating the first `k` strings in `words` for some **positive** `k` no larger than `words.length`.

Return `true` _if_ `s` _is a **prefix string** of_ `words`_, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "iloveleetcode", words = ["i","love","leetcode","apples"]

**Output:** true

**Explanation:** s can be made by concatenating "i", "love", and "leetcode" together.

**Example 2:**

**Input:** s = "iloveleetcode", words = ["apples","i","love","leetcode"]

**Output:** false

**Explanation:** It is impossible to make s using a prefix of arr.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 20`
*   `1 <= s.length <= 1000`
*   `words[i]` and `s` consist of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun isPrefixString(s: String, words: Array<String?>): Boolean {
        val sb = StringBuilder()
        for (word in words) {
            sb.append(word)
            if (sb.toString() == s) {
                return true
            }
        }
        return false
    }
}
```