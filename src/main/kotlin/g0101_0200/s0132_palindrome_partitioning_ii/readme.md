[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 132\. Palindrome Partitioning II

Hard

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return _the minimum cuts needed_ for a palindrome partitioning of `s`.

**Example 1:**

**Input:** s = "aab"

**Output:** 1

**Explanation:** The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example 2:**

**Input:** s = "a"

**Output:** 0

**Example 3:**

**Input:** s = "ab"

**Output:** 1

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lowercase English letters only.

## Solution

```kotlin
import java.util.Arrays

@Suppress("NAME_SHADOWING")
class Solution {
    fun minCut(s: String): Int {
        val n = s.length
        val t = s.toCharArray()
        val dp = IntArray(n + 1)
        Arrays.fill(dp, Int.MAX_VALUE)
        dp[0] = -1
        var i = 0
        while (i < n) {
            expandAround(t, i, i, dp)
            expandAround(t, i, i + 1, dp)
            i++
        }
        return dp[n]
    }

    private fun expandAround(t: CharArray, i: Int, j: Int, dp: IntArray) {
        var i = i
        var j = j
        while (i >= 0 && j < t.size && t[i] == t[j]) {
            dp[j + 1] = Math.min(dp[j + 1], dp[i] + 1)
            i--
            j++
        }
    }
}
```