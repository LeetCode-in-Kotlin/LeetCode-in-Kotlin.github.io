[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 943\. Find the Shortest Superstring

Hard

Given an array of strings `words`, return _the smallest string that contains each string in_ `words` _as a substring_. If there are multiple valid strings of the smallest length, return **any of them**.

You may assume that no string in `words` is a substring of another string in `words`.

**Example 1:**

**Input:** words = ["alex","loves","leetcode"]

**Output:** "alexlovesleetcode"

**Explanation:** All permutations of "alex","loves","leetcode" would also be accepted.

**Example 2:**

**Input:** words = ["catg","ctaagt","gcta","ttca","atgcatc"]

**Output:** "gctaagttcatgcatc"

**Constraints:**

*   `1 <= words.length <= 12`
*   `1 <= words[i].length <= 20`
*   `words[i]` consists of lowercase English letters.
*   All the strings of `words` are **unique**.

## Solution

```kotlin
class Solution {
    fun shortestSuperstring(words: Array<String>): String? {
        val l = words.size
        var state = 0
        for (i in 0 until l) {
            state = state or (1 shl i)
        }
        val map: MutableMap<String?, String?> = HashMap()
        return solveTPS(words, state, "", map, l)
    }

    private fun solveTPS(
        words: Array<String>,
        state: Int,
        startWord: String,
        map: MutableMap<String?, String?>,
        l: Int
    ): String? {
        val key = "$startWord|$state"
        if (state == 0) {
            return startWord
        }
        if (map[key] != null) {
            return map[key]
        }
        var minLenWord: String? = ""
        for (i in 0 until l) {
            if (state shr i and 1 == 1) {
                val takenState = state and (1 shl i).inv()
                val result = solveTPS(words, takenState, words[i], map, l)
                val tmp = mergeAndGet(startWord, result)
                if (minLenWord!!.isEmpty() || minLenWord.length > tmp.length) {
                    minLenWord = tmp
                }
            }
        }
        map[key] = minLenWord
        return minLenWord
    }

    private fun mergeAndGet(word: String, result: String?): String {
        val l = word.length
        val t = result!!.length
        if (result.contains(word)) {
            return result
        }
        if (word.contains(result)) {
            return word
        }
        var found = l
        for (k in 0 until l) {
            var i = k
            var j = 0
            while (i < l && j < t) {
                if (word[i] == result[j]) {
                    i++
                    j++
                } else break
            }
            if (i == l) {
                found = k
                break
            }
        }
        return word.substring(0, found) + result
    }
}
```