[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 139\. Word Break

Medium

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]

**Output:** true

**Explanation:** Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]

**Output:** true

**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple". 

Note that you are allowed to reuse a dictionary word.

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** false

**Constraints:**

*   `1 <= s.length <= 300`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 20`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```kotlin
import java.util.HashSet

class Solution {
    fun wordBreak(s: String, wordDict: List<String>): Boolean {
        val set: MutableSet<String> = HashSet()
        var max = 0
        val flag = BooleanArray(s.length + 1)
        for (st in wordDict) {
            set.add(st)
            if (max < st.length) {
                max = st.length
            }
        }
        for (i in 1..max) {
            if (dfs(s, 0, i, max, set, flag)) {
                return true
            }
        }
        return false
    }

    private fun dfs(s: String, start: Int, end: Int, max: Int, set: Set<String>, flag: BooleanArray): Boolean {
        if (!flag[end] && set.contains(s.substring(start, end))) {
            flag[end] = true
            if (end == s.length) {
                return true
            }
            for (i in 1..max) {
                if (end + i <= s.length && dfs(s, end, end + i, max, set, flag)) {
                    return true
                }
            }
        }
        return false
    }
}
```