[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3297\. Count Substrings That Can Be Rearranged to Contain a String I

Medium

You are given two strings `word1` and `word2`.

A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.

Return the total number of **valid** substrings of `word1`.

**Example 1:**

**Input:** word1 = "bcca", word2 = "abc"

**Output:** 1

**Explanation:**

The only valid substring is `"bcca"` which can be rearranged to `"abcc"` having `"abc"` as a prefix.

**Example 2:**

**Input:** word1 = "abcabc", word2 = "abc"

**Output:** 10

**Explanation:**

All the substrings except substrings of size 1 and size 2 are valid.

**Example 3:**

**Input:** word1 = "abcabc", word2 = "aaabc"

**Output:** 0

**Constraints:**

*   <code>1 <= word1.length <= 10<sup>5</sup></code>
*   <code>1 <= word2.length <= 10<sup>4</sup></code>
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun validSubstringCount(word1: String, word2: String): Long {
        var res: Long = 0
        var keys = 0
        val len = word1.length
        val count = IntArray(26)
        val letters = BooleanArray(26)
        for (letter in word2.toCharArray()) {
            val index = letter.code - 'a'.code
            if (count[index]++ == 0) {
                letters[index] = true
                keys++
            }
        }
        var start = 0
        var end = 0
        while (end < len) {
            val index = word1[end].code - 'a'.code
            if (!letters[index]) {
                end++
                continue
            }
            if (--count[index] == 0) {
                --keys
            }
            while (keys == 0) {
                res += (len - end).toLong()
                val beginIndex = word1[start++].code - 'a'.code
                if (!letters[beginIndex]) {
                    continue
                }
                if (count[beginIndex]++ == 0) {
                    keys++
                }
            }
            end++
        }
        return res
    }
}
```