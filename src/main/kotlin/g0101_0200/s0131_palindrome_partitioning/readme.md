[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 131\. Palindrome Partitioning

Medium

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

**Example 1:**

**Input:** s = "aab"

**Output:** [["a","a","b"],["aa","b"]]

**Example 2:**

**Input:** s = "a"

**Output:** [["a"]]

**Constraints:**

*   `1 <= s.length <= 16`
*   `s` contains only lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun partition(s: String): List<List<String>> {
        val res = ArrayList<List<String>>()
        val mem = Array(s.length) { IntArray(s.length) }

        fun isPalindrome(i: Int, j: Int): Boolean {
            if (i > j) {
                return true
            }
            return when (mem[i][j]) {
                1 -> true
                2 -> false
                else -> {
                    val res = s[i] == s[j] && isPalindrome(i + 1, j - 1)
                    mem[i][j] = if (res) 1 else 2
                    res
                }
            }
        }

        fun dfs(start: Int, path: ArrayList<String>) {
            if (start == s.length) {
                res.add(path.toList())
                return
            }

            for (i in start..s.length - 1) {
                if (!isPalindrome(start, i)) {
                    continue
                }
                path.add(s.substring(start..i))
                dfs(i + 1, path)
                path.removeAt(path.size - 1)
            }
        }
        dfs(0, ArrayList())
        return res
    }
}
```