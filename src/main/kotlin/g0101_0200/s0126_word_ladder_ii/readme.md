[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 126\. Word Ladder II

Hard

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words <code>beginWord -> s<sub>1</sub> -> s<sub>2</sub> -> ... -> s<sub>k</sub></code> such that:

*   Every adjacent pair of words differs by a single letter.
*   Every <code>s<sub>i</sub></code> for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
*   <code>s<sub>k</sub> == endWord</code>

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _all the **shortest transformation sequences** from_ `beginWord` _to_ `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ <code>[beginWord, s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub>]</code>.

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]

**Output:** [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]

**Explanation:** There are 2 shortest transformation sequences: 

"hit" -> "hot" -> "dot" -> "dog" -> "cog" 

"hit" -> "hot" -> "lot" -> "log" -> "cog"

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]

**Output:** []

**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

*   `1 <= beginWord.length <= 5`
*   `endWord.length == beginWord.length`
*   `1 <= wordList.length <= 500`
*   `wordList[i].length == beginWord.length`
*   `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
*   `beginWord != endWord`
*   All the words in `wordList` are **unique**.
*   The **sum** of all shortest transformation sequences does not exceed <code>10<sup>5</sup></code>.

## Solution

```kotlin
import java.util.Collections
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun findLadders(beginWord: String, endWord: String, wordList: List<String>): List<List<String>> {
        val ans: MutableList<List<String>> = ArrayList()
        // reverse graph start from endWord
        val reverse: MutableMap<String, MutableSet<String>> = HashMap()
        // remove the duplicate words
        val wordSet: MutableSet<String> = HashSet(wordList)
        // remove the first word to avoid cycle path
        wordSet.remove(beginWord)
        // store current layer nodes
        val queue: Queue<String> = LinkedList()
        // first layer has only beginWord
        queue.add(beginWord)
        // store nextLayer nodes
        val nextLevel: MutableSet<String> = HashSet()
        // find endWord flag
        var findEnd = false
        // traverse current layer nodes
        while (!queue.isEmpty()) {
            val word = queue.remove()
            for (next in wordSet) {
                // is ladder words
                if (isLadder(word, next)) {
                    // construct the reverse graph from endWord
                    val reverseLadders = reverse.computeIfAbsent(
                        next
                    ) { _: String? -> HashSet() }
                    reverseLadders.add(word)
                    if (endWord == next) {
                        findEnd = true
                    }
                    // store next layer nodes
                    nextLevel.add(next)
                }
            }
            // when current layer is all visited
            if (queue.isEmpty()) {
                // if find the endWord, then break the while loop
                if (findEnd) {
                    break
                }
                // add next layer nodes to queue
                queue.addAll(nextLevel)
                // remove all next layer nodes in wordSet
                wordSet.removeAll(nextLevel)
                nextLevel.clear()
            }
        }
        // if can't reach endWord from startWord, then return ans.
        if (!findEnd) {
            return ans
        }
        val path: MutableSet<String> = LinkedHashSet()
        path.add(endWord)
        // traverse reverse graph from endWord to beginWord
        findPath(endWord, beginWord, reverse, ans, path)
        return ans
    }

    private fun findPath(
        endWord: String,
        beginWord: String,
        graph: Map<String, MutableSet<String>>,
        ans: MutableList<List<String>>,
        path: MutableSet<String>
    ) {
        val next = graph[endWord] ?: return
        for (word in next) {
            path.add(word)
            if (beginWord == word) {
                val shortestPath: List<String> = ArrayList(path)
                // reverse words in shortest path
                Collections.reverse(shortestPath)
                // add the shortest path to ans.
                ans.add(shortestPath)
            } else {
                findPath(word, beginWord, graph, ans, path)
            }
            path.remove(word)
        }
    }

    private fun isLadder(s: String, t: String): Boolean {
        if (s.length != t.length) {
            return false
        }
        var diffCount = 0
        val n = s.length
        for (i in 0 until n) {
            if (s[i] != t[i]) {
                diffCount++
            }
            if (diffCount > 1) {
                return false
            }
        }
        return diffCount == 1
    }
}
```