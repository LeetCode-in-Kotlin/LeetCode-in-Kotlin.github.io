[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 953\. Verifying an Alien Dictionary

Easy

In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.

Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographically in this alien language.

**Example 1:**

**Input:** words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"

**Output:** true

**Explanation:** As 'h' comes before 'l' in this language, then the sequence is sorted.

**Example 2:**

**Input:** words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"

**Output:** false

**Explanation:** As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.

**Example 3:**

**Input:** words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"

**Output:** false

**Explanation:** The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character ([More info](https://en.wikipedia.org/wiki/Lexicographical_order)).

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 20`
*   `order.length == 26`
*   All characters in `words[i]` and `order` are English lowercase letters.

## Solution

```kotlin
class Solution {
    fun isAlienSorted(words: Array<String>, order: String): Boolean {
        val map = IntArray(26)
        for (i in order.indices) {
            map[order[i].code - 'a'.code] = i
        }
        for (i in 0 until words.size - 1) {
            if (!isSmaller(words[i], words[i + 1], map)) {
                return false
            }
        }
        return true
    }

    private fun isSmaller(str1: String, str2: String, map: IntArray): Boolean {
        val len1 = str1.length
        val len2 = str2.length
        val minLength = len1.coerceAtMost(len2)
        for (i in 0 until minLength) {
            if (map[str1[i].code - 'a'.code] > map[str2[i].code - 'a'.code]) {
                return false
            } else if (map[str1[i].code - 'a'.code] < map[str2[i].code - 'a'.code]) {
                return true
            }
        }
        return len1 <= len2
    }
}
```