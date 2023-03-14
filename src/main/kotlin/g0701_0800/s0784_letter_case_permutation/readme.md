[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 784\. Letter Case Permutation

Medium

Given a string `s`, you can transform every letter individually to be lowercase or uppercase to create another string.

Return _a list of all possible strings we could create_. Return the output in **any order**.

**Example 1:**

**Input:** s = "a1b2"

**Output:** ["a1b2","a1B2","A1b2","A1B2"]

**Example 2:**

**Input:** s = "3z4"

**Output:** ["3z4","3Z4"]

**Constraints:**

*   `1 <= s.length <= 12`
*   `s` consists of lowercase English letters, uppercase English letters, and digits.

## Solution

```kotlin
import java.util.Locale

class Solution {
    private val ans: MutableList<String> = ArrayList()

    fun letterCasePermutation(s: String): List<String> {
        helper(s, 0, "")
        return ans
    }

    private fun helper(s: String, curr: Int, temp: String) {
        if (curr == s.length) {
            ans.add(temp)
            return
        }
        if (Character.isDigit(s[curr])) {
            helper(s, curr + 1, temp + s[curr])
        } else {
            if (Character.isLowerCase(s[curr])) {
                helper(s, curr + 1, temp + s[curr])
                helper(s, curr + 1, temp + s.substring(curr, curr + 1).uppercase(Locale.getDefault()))
            } else {
                helper(s, curr + 1, temp + s[curr])
                helper(s, curr + 1, temp + s.substring(curr, curr + 1).lowercase(Locale.getDefault()))
            }
        }
    }
}
```