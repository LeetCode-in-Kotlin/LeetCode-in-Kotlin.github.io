[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 720\. Longest Word in Dictionary

Medium

Given an array of strings `words` representing an English Dictionary, return _the longest word in_ `words` _that can be built one character at a time by other words in_ `words`.

If there is more than one possible answer, return the longest word with the smallest lexicographical order. If there is no answer, return the empty string.

Note that the word should be built from left to right with each additional character being added to the end of a previous word.

**Example 1:**

**Input:** words = ["w","wo","wor","worl","world"]

**Output:** "world"

**Explanation:** The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".

**Example 2:**

**Input:** words = ["a","banana","app","appl","ap","apply","apple"]

**Output:** "apple"

**Explanation:** Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 30`
*   `words[i]` consists of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private val root = Node()
    private var longestWord: String? = ""

    private class Node {
        var children: Array<Node?> = arrayOfNulls(26)
        var str: String? = null

        fun insert(curr: Node?, word: String) {
            var curr = curr
            for (element in word) {
                if (curr!!.children[element.code - 'a'.code] == null) {
                    curr.children[element.code - 'a'.code] = Node()
                }
                curr = curr.children[element.code - 'a'.code]
            }
            curr!!.str = word
        }
    }

    fun longestWord(words: Array<String>): String? {
        for (word in words) {
            root.insert(root, word)
        }
        dfs(root)
        return longestWord
    }

    private fun dfs(curr: Node?) {
        for (i in 0..25) {
            if (curr!!.children[i] != null && curr.children[i]!!.str != null) {
                if (curr.children[i]!!.str!!.length > longestWord!!.length) {
                    longestWord = curr.children[i]!!.str
                }
                dfs(curr.children[i])
            }
        }
    }
}
```