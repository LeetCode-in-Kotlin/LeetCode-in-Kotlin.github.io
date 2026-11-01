[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3926\. Count Valid Word Occurrences

Medium

You are given an array of strings `chunks`. Concatenate all strings in `chunks` in order to form a string `s`.

You are also given an array of strings `queries`.

A **joiner hyphen** is a hyphen character `'-'` in `s` whose previous and next characters both exist and are lowercase English letters.

A **word** is a **maximal** substring of `s` consisting only of lowercase English letters and **joiner hyphens**.

All other characters, including spaces and hyphens that are not **joiner hyphens**, are treated as separators.

Return an integer array `ans`, where `ans[i]` is the number of times `queries[i]` appears as a word in `s`.

**Example 1:**

**Input:** chunks = ["hello wor","ld hello"], queries = ["hello","world","wor"]

**Output:** [2,1,0]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "hello world hello"`.
*   The words are `"hello"`, `"world"`, and `"hello"`.
*   The substring `"wor"` appears inside `"world"`, but it is not a full word.

**Example 2:**

**Input:** chunks = ["a-b a--b ","a-","b"], queries = ["a-b","a","b"]

**Output:** [2,1,1]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "a-b a--b a-b"`.
*   In `"a-b"`, the hyphen is a joiner hyphen because it is between two lowercase English letters, so `"a-b"` is one word.
*   In `"a--b"`, neither hyphen is a joiner hyphen, so it is split into the words `"a"` and `"b"`.
*   Therefore, the words are `"a-b"`, `"a"`, `"b"`, and `"a-b"`.

**Example 3:**

**Input:** chunks = ["-cat dog- mouse"], queries = ["cat","dog","mouse","cat-dog"]

**Output:** [1,1,1,0]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "-cat dog- mouse"`.
*   The leading hyphen before `"cat"` and the trailing hyphen after `"dog"` are not joiner hyphens, so they are separators.
*   The words are `"cat"`, `"dog"`, and `"mouse"`.

**Constraints:**

*   <code>1 <= chunks.length <= 10<sup>5</sup></code>
*   <code>1 <= chunks[i].length <= 10<sup>5</sup></code>
*   The total length of all strings in `chunks` does not exceed <code>10<sup>5</sup></code>.
*   `chunks[i]` consists only of lowercase English letters, spaces, and `'-'`.
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>1 <= queries[i].length <= 10<sup>5</sup></code>
*   The total length of all strings in `queries` does not exceed <code>10<sup>5</sup></code>.
*   `queries[i]` consists only of lowercase English letters and `'-'`.
*   `queries[i]` is a valid word: it does not start or end with `'-'`, and it does not contain two consecutive hyphens.

## Solution

```kotlin
class Solution {
    fun countWordOccurrences(chunks: Array<String>, queries: Array<String>): IntArray {
        val trie = Trie()
        trie.fill(chunks)
        return queries.map { trie.count(it) }.toIntArray()
    }

    private class Trie {
        val children: Array<Trie?> = arrayOfNulls(27)
        var count: Int = 0

        fun fill(chunks: Array<String>) {
            var node = this
            var last = '@'
            for (j in chunks.indices) {
                val chunk = chunks[j]
                for (k in chunk.indices) {
                    val c = processCharacter(chunk, k, j, chunks, last)
                    node = node.addCharacter(c)
                    node = node.finalizeWordIfNeeded(c, this)
                    last = c
                }
            }
            node.incrementCountIfNeeded(this)
        }

        private fun processCharacter(
            chunk: String,
            index: Int,
            chunkIndex: Int,
            chunks: Array<String>,
            last: Char,
        ): Char {
            val c = chunk[index]
            if (c == '-') {
                val next = getNextCharacter(chunk, index, chunkIndex, chunks)
                if (!isLetter(last) || !isLetter(next)) {
                    return '@'
                }
            }
            return c
        }

        private fun getNextCharacter(chunk: String, index: Int, chunkIndex: Int, chunks: Array<String>): Char {
            if (index < chunk.length - 1) {
                return chunk[index + 1]
            }
            if (chunkIndex < chunks.size - 1) {
                return chunks[chunkIndex + 1][0]
            }
            return '@'
        }

        private fun addCharacter(c: Char): Trie {
            if (!isValidCharacter(c)) {
                return this
            }
            val index = getIndex(c)
            if (children[index] == null) {
                children[index] = Trie()
            }
            return children[index]!!
        }

        private fun finalizeWordIfNeeded(c: Char, root: Trie): Trie {
            if (isValidCharacter(c)) {
                return this
            }
            incrementCountIfNeeded(root)
            return root
        }

        private fun incrementCountIfNeeded(root: Trie) {
            if (this !== root) {
                count++
            }
        }

        fun count(s: String): Int {
            var node: Trie? = this
            for (element in s) {
                val i = getIndex(element)
                node = node?.children?.get(i) ?: return 0
            }
            return node?.count ?: 0
        }

        companion object {
            private fun isValidCharacter(c: Char): Boolean = c == '-' || isLetter(c)

            private fun getIndex(c: Char): Int = if (c == '-') 26 else c - 'a'

            private fun isLetter(c: Char): Boolean = c in 'a'..'z'
        }
    }
}
```