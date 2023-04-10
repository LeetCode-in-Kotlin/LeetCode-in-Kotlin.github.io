[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 890\. Find and Replace Pattern

Medium

Given a list of strings `words` and a string `pattern`, return _a list of_ `words[i]` _that match_ `pattern`. You may return the answer in **any order**.

A word matches the pattern if there exists a permutation of letters `p` so that after replacing every letter `x` in the pattern with `p(x)`, we get the desired word.

Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.

**Example 1:**

**Input:** words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"

**Output:** ["mee","aqq"]

**Explanation:** "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}. "ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation, since a and b map to the same letter.

**Example 2:**

**Input:** words = ["a","b","c"], pattern = "a"

**Output:** ["a","b","c"]

**Constraints:**

*   `1 <= pattern.length <= 20`
*   `1 <= words.length <= 50`
*   `words[i].length == pattern.length`
*   `pattern` and `words[i]` are lowercase English letters.

## Solution

```kotlin
import java.util.Collections

class Solution {
    fun findAndReplacePattern(words: Array<String>, pattern: String): List<String> {
        val finalans: MutableList<String> = ArrayList()
        if (pattern.length == 1) {
            Collections.addAll(finalans, *words)
            return finalans
        }
        for (word in words) {
            val check = CharArray(26)
            check.fill('1')
            val ans: HashMap<Char, Char> = HashMap()
            for (j in word.indices) {
                val pat = pattern[j]
                val wor = word[j]
                if (ans.containsKey(pat)) {
                    if (ans[pat] == wor) {
                        if (j == word.length - 1) {
                            finalans.add(word)
                        }
                    } else {
                        break
                    }
                } else {
                    if (j == word.length - 1 && check[wor.code - 'a'.code] == '1') {
                        finalans.add(word)
                    }
                    if (check[wor.code - 'a'.code] != '1' && check[wor.code - 'a'.code] != pat) {
                        break
                    }
                    if (check[wor.code - 'a'.code] == '1') {
                        ans[pat] = wor
                        check[wor.code - 'a'.code] = pat
                    }
                }
            }
        }
        return finalans
    }
}
```