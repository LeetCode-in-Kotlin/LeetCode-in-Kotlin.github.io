[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 318\. Maximum Product of Word Lengths

Medium

Given a string array `words`, return _the maximum value of_ `length(word[i]) * length(word[j])` _where the two words do not share common letters_. If no such two words exist, return `0`.

**Example 1:**

**Input:** words = ["abcw","baz","foo","bar","xtfn","abcdef"]

**Output:** 16

**Explanation:** The two words can be "abcw", "xtfn".

**Example 2:**

**Input:** words = ["a","ab","abc","d","cd","bcd","abcd"]

**Output:** 4

**Explanation:** The two words can be "ab", "cd".

**Example 3:**

**Input:** words = ["a","aa","aaa","aaaa"]

**Output:** 0

**Explanation:** No such pair of words.

**Constraints:**

*   `2 <= words.length <= 1000`
*   `1 <= words[i].length <= 1000`
*   `words[i]` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun maxProduct(words: Array<String>): Int {
        val n = words.size
        var res = 0
        val masks = IntArray(n)
        for (i in 0 until n) {
            masks[i] = getMask(words[i])
        }
        for (i in 0 until n) {
            for (j in i + 1 until n) {
                if (masks[i] and masks[j] == 0) {
                    res = Math.max(res, words[i].length * words[j].length)
                }
            }
        }
        return res
    }

    private fun getMask(s: String): Int {
        var mask = 0
        for (i in 0 until s.length) {
            val c = s[i]
            mask = mask or (1 shl c.code - 'a'.code)
        }
        return mask
    }
}
```