[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 336\. Palindrome Pairs

Hard

You are given a **0-indexed** array of **unique** strings `words`.

A **palindrome pair** is a pair of integers `(i, j)` such that:

*   `0 <= i, j < word.length`,
*   `i != j`, and
*   `words[i] + words[j]` (the concatenation of the two strings) is a palindrome.

Return _an array of all the **palindrome pairs** of_ `words`.

**Example 1:**

**Input:** words = ["abcd","dcba","lls","s","sssll"]

**Output:** [[0,1],[1,0],[3,2],[2,4]]

**Explanation:** The palindromes are ["abcddcba","dcbaabcd","slls","llssssll"]

**Example 2:**

**Input:** words = ["bat","tab","cat"]

**Output:** [[0,1],[1,0]]

**Explanation:** The palindromes are ["battab","tabbat"]

**Example 3:**

**Input:** words = ["a",""]

**Output:** [[0,1],[1,0]]

**Explanation:** The palindromes are ["a","a"]

**Constraints:**

*   `1 <= words.length <= 5000`
*   `0 <= words[i].length <= 300`
*   `words[i]` consists of lowercase English letters.

## Solution

```kotlin
@Suppress("kotlin:S6218", "NAME_SHADOWING")
class Solution {
    fun palindromePairs(words: Array<String>): List<List<Int>> {
        val ans = mutableListOf<List<Int>>()
        val root = TrieNode()
        for (idx in words.indices) {
            addWord(root, words[idx], idx)
        }
        for (idx in words.indices) {
            search(idx, root, words, ans)
        }
        return ans
    }

    private fun search(idxCurWord: Int, root: TrieNode, words: Array<String>, res: MutableList<List<Int>>) {
        var cur: TrieNode? = root
        val curWord = words[idxCurWord]
        val lenW = curWord.length
        for (idxCh in curWord.indices) {
            if (cur!!.index >= 0 && cur.index != idxCurWord && isPalindrome(curWord, idxCh, lenW - 1))
                res.add(listOf(idxCurWord, cur.index))
            cur = cur.children[curWord[idxCh] - 'a']
            if (cur == null)
                return
        }
        for (idxPalin in cur!!.panlinIndicies) {
            if (idxPalin == idxCurWord) continue
            res.add(listOf(idxCurWord, idxPalin))
        }
    }

    private fun addWord(root: TrieNode, word: String, index: Int) {
        var cur: TrieNode? = root
        for (idx in word.indices.reversed()) {
            val idxCh = word[idx] - 'a'
            if (cur!!.children[idxCh] == null)
                cur.children[idxCh] = TrieNode()
            if (isPalindrome(word, 0, idx))
                cur.panlinIndicies.add(index)
            cur = cur.children[idxCh]
        }
        cur!!.panlinIndicies.add(index)
        cur.index = index
    }

    private fun isPalindrome(word: String, lo: Int, hi: Int): Boolean {
        var lo = lo
        var hi = hi
        while (lo < hi) {
            if (word[lo] != word[hi])
                return false
            ++lo
            --hi
        }
        return true
    }

    private data class TrieNode(
        val children: Array<TrieNode?> = Array<TrieNode?>(26) { null },
        var index: Int = -1,
        val panlinIndicies: MutableList<Int> = mutableListOf<Int>()
    )
}
```