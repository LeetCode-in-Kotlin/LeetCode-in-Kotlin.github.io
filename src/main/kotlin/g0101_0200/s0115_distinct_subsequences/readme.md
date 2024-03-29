[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 115\. Distinct Subsequences

Hard

Given two strings `s` and `t`, return _the number of distinct subsequences of `s` which equals `t`_.

A string's **subsequence** is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e., `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example 1:**

**Input:** s = "rabbbit", t = "rabbit"

**Output:** 3

**Explanation:** As shown below, there are 3 ways you can generate "rabbit" from s. <code>**<ins>rabb</ins>**b**<ins>it</ins>**</code> <code>**<ins>ra</ins>**b**<ins>bbit</ins>**</code> <code>**<ins>rab</ins>**b**<ins>bit</ins>**</code>

**Example 2:**

**Input:** s = "babgbag", t = "bag"

**Output:** 5

**Explanation:** As shown below, there are 5 ways you can generate "bag" from s. <code>**<ins>ba</ins>**b<ins>**g**</ins>bag</code> <code>**<ins>ba</ins>**bgba**<ins>g</ins>**</code> <code><ins>**b**</ins>abgb**<ins>ag</ins>**</code> <code>ba<ins>**b**</ins>gb<ins>**ag**</ins></code> <code>babg**<ins>bag</ins>**</code>

**Constraints:**

*   `1 <= s.length, t.length <= 1000`
*   `s` and `t` consist of English letters.

## Solution

```kotlin
class Solution {
    fun numDistinct(s: String, t: String): Int {
        if (s.length < t.length) {
            return 0
        }
        if (s.length == t.length) {
            return if (s == t) 1 else 0
        }
        val move = s.length - t.length + 2
        // Only finite number of character in s can occupy first position in T. Same applies for
        // every character in T.
        val dp = IntArray(move)
        var j = 1
        var k = 1
        for (i in 0 until t.length) {
            var firstMatch = true
            while (j < move) {
                if (t[i] == s[i + j - 1]) {
                    if (firstMatch) {
                        // Keep track of first match. To avoid useless comparisons on next
                        // iteration.
                        k = j
                        firstMatch = false
                    }
                    if (i == 0) {
                        dp[j] = 1
                    }
                    dp[j] += dp[j - 1]
                } else {
                    dp[j] = dp[j - 1]
                }
                j++
            }
            // No match found for current character of t in s. No point in checking others.
            if (dp[move - 1] == 0) {
                return 0
            }
            j = k
        }
        return dp[move - 1]
    }
}
```