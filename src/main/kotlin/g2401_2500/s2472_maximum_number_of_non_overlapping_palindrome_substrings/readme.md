[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2472\. Maximum Number of Non-overlapping Palindrome Substrings

Hard

You are given a string `s` and a **positive** integer `k`.

Select a set of **non-overlapping** substrings from the string `s` that satisfy the following conditions:

*   The **length** of each substring is **at least** `k`.
*   Each substring is a **palindrome**.

Return _the **maximum** number of substrings in an optimal selection_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "abaccdbbd", k = 3

**Output:** 2

**Explanation:** We can select the substrings underlined in s = "<ins>**aba**</ins>cc<ins>**dbbd**</ins>". Both "aba" and "dbbd" are palindromes and have a length of at least k = 3. It can be shown that we cannot find a selection with more than two valid substrings.

**Example 2:**

**Input:** s = "adbcda", k = 2

**Output:** 0

**Explanation:** There is no palindrome substring of length at least 2 in the string.

**Constraints:**

*   `1 <= k <= s.length <= 2000`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxPalindromes(s: String, k: Int): Int {
        val dp = IntArray(s.length)
        dp.fill(-1)
        return dfs(s, dp, k, 0)
    }

    private fun dfs(s: String, dp: IntArray, k: Int, start: Int): Int {
        if (start >= s.length) {
            return 0
        }
        if (dp[start] != -1) {
            return dp[start]
        }
        var ans = 0
        for (n in 0..1) {
            for (i in start..s.length - k - n) {
                var left = i
                var right = i + k + n - 1
                while (left < right) {
                    if (s[left] != s[right]) {
                        break
                    }
                    left++
                    right--
                }
                if (left >= right) {
                    ans = Math.max(ans, 1 + dfs(s, dp, k, i + k + n))
                    break
                }
            }
        }
        dp[start] = ans
        return ans
    }
}
```