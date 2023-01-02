[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 472\. Concatenated Words

Hard

Given an array of strings `words` (**without duplicates**), return _all the **concatenated words** in the given list of_ `words`.

A **concatenated word** is defined as a string that is comprised entirely of at least two shorter words in the given array.

**Example 1:**

**Input:** words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]

**Output:** ["catsdogcats","dogcatsdog","ratcatdogcat"]

**Explanation:** "catsdogcats" can be concatenated by "cats", "dog" and "cats"; "dogcatsdog" can be concatenated by "dog", "cats" and "dog"; "ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".

**Example 2:**

**Input:** words = ["cat","dog","catdog"]

**Output:** ["catdog"]

**Constraints:**

*   <code>1 <= words.length <= 10<sup>4</sup></code>
*   `1 <= words[i].length <= 30`
*   `words[i]` consists of only lowercase English letters.
*   All the strings of `words` are **unique**.
*   <code>1 <= sum(words[i].length) <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    private val ans: MutableList<String> = ArrayList()
    private var root: Trie? = null
    fun findAllConcatenatedWordsInADict(words: Array<String>): List<String> {
        root = Trie()
        Arrays.sort(
            words
        ) { a: String, b: String ->
            Integer.compare(
                a.length,
                b.length
            )
        }
        for (word in words) {
            var ptr = root
            if (search(word, 0, 0)) {
                ans.add(word)
            } else {
                for (j in 0 until word.length) {
                    if (ptr!!.nxt[word[j].code - 'a'.code] == null) {
                        ptr.nxt[word[j].code - 'a'.code] = Trie()
                    }
                    ptr = ptr.nxt[word[j].code - 'a'.code]
                }
                ptr!!.endHere = true
            }
        }
        return ans
    }

    private fun search(cur: String, idx: Int, wordCnt: Int): Boolean {
        if (idx == cur.length) {
            return wordCnt >= 2
        }
        var ptr = root
        for (i in idx until cur.length) {
            if (ptr!!.nxt[cur[i].code - 'a'.code] == null) {
                return false
            }
            if (ptr.nxt[cur[i].code - 'a'.code]!!.endHere) {
                val ret = search(cur, i + 1, wordCnt + 1)
                if (ret) {
                    return true
                }
            }
            ptr = ptr.nxt[cur[i].code - 'a'.code]
        }
        return ptr!!.endHere && wordCnt >= 2
    }

    private class Trie internal constructor() {
        var nxt: Array<Trie?>
        var endHere: Boolean

        init {
            nxt = arrayOfNulls(26)
            endHere = false
        }
    }
}
```