[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1081\. Smallest Subsequence of Distinct Characters

Medium

Given a string `s`, return _the_ _lexicographically smallest_ _subsequence_ _of_ `s` _that contains all the distinct characters of_ `s` _exactly once_.

**Example 1:**

**Input:** s = "bcabc"

**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"

**Output:** "acdb"

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

**Note:** This question is the same as 316: [https://leetcode.com/problems/remove-duplicate-letters/](https://leetcode.com/problems/remove-duplicate-letters/)

## Solution

```kotlin
import java.util.Arrays
import java.util.Deque
import java.util.LinkedList

class Solution {
    fun smallestSubsequence(s: String): String {
        val n = s.length
        val stk: Deque<Char> = LinkedList()
        val freq = IntArray(26)
        val exist = BooleanArray(26)
        Arrays.fill(exist, false)
        for (ch in s.toCharArray()) {
            freq[ch.code - 'a'.code]++
        }
        for (i in 0 until n) {
            val ch = s[i]
            freq[ch.code - 'a'.code]--
            if (exist[ch.code - 'a'.code]) {
                continue
            }
            while (stk.isNotEmpty() && stk.peek() > ch && freq[stk.peek().code - 'a'.code] > 0) {
                val rem = stk.pop()
                exist[rem.code - 'a'.code] = false
            }
            stk.push(ch)
            exist[ch.code - 'a'.code] = true
        }
        val ans = CharArray(stk.size)
        var index = 0
        while (stk.isNotEmpty()) {
            ans[index] = stk.pop()
            index++
        }
        return StringBuilder(String(ans)).reverse().toString()
    }
}
```