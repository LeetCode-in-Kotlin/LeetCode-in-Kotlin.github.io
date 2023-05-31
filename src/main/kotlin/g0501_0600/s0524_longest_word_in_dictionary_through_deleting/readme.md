[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 524\. Longest Word in Dictionary through Deleting

Medium

Given a string `s` and a string array `dictionary`, return _the longest string in the dictionary that can be formed by deleting some of the given string characters_. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

**Example 1:**

**Input:** s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]

**Output:** "apple"

**Example 2:**

**Input:** s = "abpcplea", dictionary = ["a","b","c"]

**Output:** "a"

**Constraints:**

*   `1 <= s.length <= 1000`
*   `1 <= dictionary.length <= 1000`
*   `1 <= dictionary[i].length <= 1000`
*   `s` and `dictionary[i]` consist of lowercase English letters.

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

class Solution {
    private class Pair(var word: String, var index: Int)

    fun findLongestWord(s: String, dictionary: List<String>): String {
        val map: MutableMap<Char, Deque<Pair?>> = HashMap()
        var c = 'a'
        while (c <= 'z') {
            map[c] = ArrayDeque()
            c++
        }
        for (word in dictionary) {
            map[word[0]]!!.offerFirst(Pair(word, 0))
        }
        var maxLen = 0
        var res = ""
        for (i in 0 until s.length) {
            if (map[s[i]]!!.isNotEmpty()) {
                val deque = map[s[i]]!!
                val size = deque.size
                for (j in 0 until size) {
                    val pair = deque.pollLast()!!
                    if (pair.index == pair.word.length - 1) {
                        if (maxLen < pair.word.length) {
                            maxLen = pair.word.length
                            res = pair.word
                        } else if (maxLen == pair.word.length && res.compareTo(pair.word) > 0) {
                            res = pair.word
                        }
                    } else {
                        pair.index++
                        map[pair.word[pair.index]]!!.offerFirst(pair)
                    }
                }
            }
        }
        return res
    }
}
```