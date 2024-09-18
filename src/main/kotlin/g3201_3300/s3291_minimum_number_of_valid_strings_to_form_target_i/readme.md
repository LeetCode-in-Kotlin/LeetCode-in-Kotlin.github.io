[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3291\. Minimum Number of Valid Strings to Form Target I

Medium

You are given an array of strings `words` and a string `target`.

A string `x` is called **valid** if `x` is a prefix of **any** string in `words`.

Return the **minimum** number of **valid** strings that can be _concatenated_ to form `target`. If it is **not** possible to form `target`, return `-1`.

A prefix of a string is a substring that starts from the beginning of the string and extends to any point within it.

**Example 1:**

**Input:** words = ["abc","aaaaa","bcdef"], target = "aabcdabc"

**Output:** 3

**Explanation:**

The target string can be formed by concatenating:

*   Prefix of length 2 of `words[1]`, i.e. `"aa"`.
*   Prefix of length 3 of `words[2]`, i.e. `"bcd"`.
*   Prefix of length 3 of `words[0]`, i.e. `"abc"`.

**Example 2:**

**Input:** words = ["abababab","ab"], target = "ababaababa"

**Output:** 2

**Explanation:**

The target string can be formed by concatenating:

*   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.
*   Prefix of length 5 of `words[0]`, i.e. `"ababa"`.

**Example 3:**

**Input:** words = ["abcdef"], target = "xyz"

**Output:** \-1

**Constraints:**

*   `1 <= words.length <= 100`
*   <code>1 <= words[i].length <= 5 * 10<sup>3</sup></code>
*   The input is generated such that <code>sum(words[i].length) <= 10<sup>5</sup></code>.
*   `words[i]` consists only of lowercase English letters.
*   <code>1 <= target.length <= 5 * 10<sup>3</sup></code>
*   `target` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minValidStrings(words: Array<String>, target: String): Int {
        val root = TrieNode()
        for (word in words) {
            insert(root, word)
        }
        val n = target.length
        val dp = IntArray(n)
        for (i in n - 1 downTo 0) {
            dp[i] = Int.Companion.MAX_VALUE
            var node = root
            for (j in i until n) {
                val idx = target[j].code - 'a'.code
                if (node.children[idx] == null) {
                    break
                }
                if (j == n - 1) {
                    dp[i] = 1
                } else if (dp[j + 1] >= 0) {
                    dp[i] = min(dp[i], (1 + dp[j + 1]))
                }
                node = node.children[idx]!!
            }
            if (dp[i] == Int.Companion.MAX_VALUE) {
                dp[i] = -1
            }
        }
        return dp[0]
    }

    private fun insert(root: TrieNode, word: String) {
        var node = root
        for (c in word.toCharArray()) {
            if (node.children[c.code - 'a'.code] == null) {
                node.children[c.code - 'a'.code] = TrieNode()
            }
            node = node.children[c.code - 'a'.code]!!
        }
    }

    private class TrieNode {
        var children: Array<TrieNode?> = arrayOfNulls<TrieNode>(26)
    }
}
```