[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 72\. Edit Distance

Hard

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

*   Insert a character
*   Delete a character
*   Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"

**Output:** 3

**Explanation:** 

horse -> rorse (replace 'h' with 'r') 

rorse -> rose (remove 'r') 

rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"

**Output:** 5

**Explanation:** 

intention -> inention (remove 't') 

inention -> enention (replace 'i' with 'e') 

enention -> exention (replace 'n' with 'x') 

exention -> exection (replace 'n' with 'c') 

exection -> execution (insert 'u')

**Constraints:**

*   `0 <= word1.length, word2.length <= 500`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minDistance(word1: String, word2: String): Int {
        val n1 = word1.length
        val n2 = word2.length
        if (n2 > n1) {
            return minDistance(word2, word1)
        }
        val dp = IntArray(n2 + 1)
        for (j in 0..n2) {
            dp[j] = j
        }
        for (i in 1..n1) {
            var pre = dp[0]
            dp[0] = i
            for (j in 1..n2) {
                val tmp = dp[j]
                dp[j] = if (word1[i - 1] != word2[j - 1]) 1 + Math.min(pre, Math.min(dp[j], dp[j - 1])) else pre
                pre = tmp
            }
        }
        return dp[n2]
    }
}
```