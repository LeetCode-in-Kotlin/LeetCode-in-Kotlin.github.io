[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 520\. Detect Capital

Easy

We define the usage of capitals in a word to be right when one of the following cases holds:

*   All letters in this word are capitals, like `"USA"`.
*   All letters in this word are not capitals, like `"leetcode"`.
*   Only the first letter in this word is capital, like `"Google"`.

Given a string `word`, return `true` if the usage of capitals in it is right.

**Example 1:**

**Input:** word = "USA"

**Output:** true

**Example 2:**

**Input:** word = "FlaG"

**Output:** false

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase and uppercase English letters.

## Solution

```kotlin
class Solution {
    fun detectCapitalUse(word: String): Boolean {
        if (word.isEmpty()) {
            return false
        }
        var upper = 0
        var lower = 0
        val n = word.length
        var firstUpper = Character.isUpperCase(word[0])
        for (i in 0 until n) {
            if (Character.isUpperCase(word[i])) {
                upper++
            } else if (Character.isLowerCase(word[i])) {
                lower++
            }
        }
        if (firstUpper && upper > 1) {
            firstUpper = false
        }
        return upper == n || lower == n || firstUpper
    }
}
```