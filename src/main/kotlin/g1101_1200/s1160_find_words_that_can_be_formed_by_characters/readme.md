[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1160\. Find Words That Can Be Formed by Characters

Easy

You are given an array of strings `words` and a string `chars`.

A string is **good** if it can be formed by characters from chars (each character can only be used once).

Return _the sum of lengths of all good strings in words_.

**Example 1:**

**Input:** words = ["cat","bt","hat","tree"], chars = "atach"

**Output:** 6

**Explanation:** The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.

**Example 2:**

**Input:** words = ["hello","world","leetcode"], chars = "welldonehoneyr"

**Output:** 10

**Explanation:** The strings that can be formed are "hello" and "world" so the answer is 5 + 5 = 10.

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length, chars.length <= 100`
*   `words[i]` and `chars` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun countCharacters(words: Array<String>, chars: String): Int {
        var length = 0
        val map: MutableMap<Char, Int> = HashMap()
        for (c in chars.toCharArray()) {
            val count = map.getOrDefault(c, 0)
            map[c] = count + 1
        }
        for (word in words) {
            if (canForm(word, map)) {
                length += word.length
            }
        }
        return length
    }

    private fun canForm(word: String, map: Map<Char, Int>): Boolean {
        val tmp: MutableMap<Char, Int> = HashMap(map)
        for (c in word.toCharArray()) {
            if (tmp.containsKey(c) && tmp.getValue(c) > 0) {
                val count = tmp.getValue(c)
                tmp[c] = count - 1
            } else {
                return false
            }
        }
        return true
    }
}
```