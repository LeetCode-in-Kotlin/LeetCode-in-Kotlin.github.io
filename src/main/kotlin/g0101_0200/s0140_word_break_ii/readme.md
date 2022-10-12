[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 140\. Word Break II

Hard

Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]

**Output:** ["cats and dog","cat sand dog"]

**Example 2:**

**Input:** s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]

**Output:** ["pine apple pen apple","pineapple pen apple","pine applepen apple"]

**Explanation:** Note that you are allowed to reuse a dictionary word.

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** []

**Constraints:**

*   `1 <= s.length <= 20`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 10`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```kotlin
import java.util.LinkedList

class Solution {
    fun wordBreak(s: String, wordDict: List<String>?): List<String> {
        val result: MutableList<String> = LinkedList()
        val wordSet: Set<String> = HashSet(wordDict)
        dfs(s, wordSet, 0, StringBuilder(), result)
        return result
    }

    private fun dfs(
        s: String,
        wordSet: Set<String>,
        index: Int,
        sb: StringBuilder,
        result: MutableList<String>
    ) {
        if (index == s.length) {
            if (sb[sb.length - 1] == ' ') {
                sb.setLength(sb.length - 1)
            }
            result.add(sb.toString())
            return
        }
        val len = sb.length
        for (i in index + 1..s.length) {
            val subs = s.substring(index, i)
            if (wordSet.contains(subs)) {
                sb.append(subs).append(" ")
                dfs(s, wordSet, i, sb, result)
            }
            sb.setLength(len)
        }
    }
}
```