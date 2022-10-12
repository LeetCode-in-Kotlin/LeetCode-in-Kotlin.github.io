[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 127\. Word Ladder

Hard

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words <code>beginWord -> s<sub>1</sub> -> s<sub>2</sub> -> ... -> s<sub>k</sub></code> such that:

*   Every adjacent pair of words differs by a single letter.
*   Every <code>s<sub>i</sub></code> for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
*   <code>s<sub>k</sub> == endWord</code>

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]

**Output:** 5

**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]

**Output:** 0

**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

*   `1 <= beginWord.length <= 10`
*   `endWord.length == beginWord.length`
*   `1 <= wordList.length <= 5000`
*   `wordList[i].length == beginWord.length`
*   `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
*   `beginWord != endWord`
*   All the words in `wordList` are **unique**.

## Solution

```kotlin
class Solution {
    fun ladderLength(beginWord: String, endWord: String, wordDict: List<String>): Int {
        var beginSet: MutableSet<String> = HashSet()
        var endSet: MutableSet<String> = HashSet()
        val wordSet: Set<String> = HashSet(wordDict)
        val visited: MutableSet<String> = HashSet()
        if (!wordDict.contains(endWord)) {
            return 0
        }
        var len = 1
        val strLen = beginWord.length
        beginSet.add(beginWord)
        endSet.add(endWord)
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            if (beginSet.size > endSet.size) {
                val temp = beginSet
                beginSet = endSet
                endSet = temp
            }
            val tempSet: MutableSet<String> = HashSet()
            for (s in beginSet) {
                val chars = s.toCharArray()
                for (i in 0 until strLen) {
                    val old = chars[i]
                    var j = 'a'
                    while (j <= 'z') {
                        chars[i] = j
                        val temp = String(chars)
                        if (endSet.contains(temp)) {
                            return len + 1
                        }
                        if (!visited.contains(temp) && wordSet.contains(temp)) {
                            tempSet.add(temp)
                            visited.add(temp)
                        }
                        j++
                    }
                    chars[i] = old
                }
            }
            beginSet = tempSet
            len++
        }
        return 0
    }
}
```