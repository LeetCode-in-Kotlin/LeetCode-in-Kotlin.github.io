[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 395\. Longest Substring with At Least K Repeating Characters

Medium

Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _such that the frequency of each character in this substring is greater than or equal to_ `k`.

**Example 1:**

**Input:** s = "aaabb", k = 3

**Output:** 3

**Explanation:** The longest substring is "aaa", as 'a' is repeated 3 times.

**Example 2:**

**Input:** s = "ababbc", k = 2

**Output:** 5

**Explanation:** The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of only lowercase English letters.
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun longestSubstring(s: String, k: Int): Int {
        return helper(s, k, 0, s.length)
    }

    private fun helper(s: String, k: Int, start: Int, end: Int): Int {
        if (end - start < k) {
            return 0
        }
        val nums = IntArray(26)
        for (i in start until end) {
            nums[s[i].code - 'a'.code]++
        }
        for (i in start until end) {
            if (nums[s[i].code - 'a'.code] < k) {
                var j = i + 1
                while (j < s.length && nums[s[j].code - 'a'.code] < k) {
                    j++
                }
                return Math.max(helper(s, k, start, i), helper(s, k, j, end))
            }
        }
        return end - start
    }
}
```