[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 290\. Word Pattern

Easy

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`.

**Example 1:**

**Input:** pattern = "abba", s = "dog cat cat dog"

**Output:** true

**Example 2:**

**Input:** pattern = "abba", s = "dog cat cat fish"

**Output:** false

**Example 3:**

**Input:** pattern = "aaaa", s = "dog cat cat dog"

**Output:** false

**Constraints:**

*   `1 <= pattern.length <= 300`
*   `pattern` contains only lower-case English letters.
*   `1 <= s.length <= 3000`
*   `s` contains only lowercase English letters and spaces `' '`.
*   `s` **does not contain** any leading or trailing spaces.
*   All the words in `s` are separated by a **single space**.

## Solution

```kotlin
class Solution {
    fun wordPattern(pattern: String, s: String): Boolean {
        val map: MutableMap<Char, String> = HashMap()
        val words = s.split(" ".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        if (words.size != pattern.length) {
            return false
        }
        for (i in 0 until pattern.length) {
            if (!map.containsKey(pattern[i])) {
                if (map.containsValue(words[i])) {
                    return false
                }
                map[pattern[i]] = words[i]
            } else {
                if (words[i] != map[pattern[i]]) {
                    return false
                }
            }
        }
        return true
    }
}
```