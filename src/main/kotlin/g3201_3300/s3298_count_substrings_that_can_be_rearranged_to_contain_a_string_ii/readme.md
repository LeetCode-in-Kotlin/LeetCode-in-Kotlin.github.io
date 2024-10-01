[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3298\. Count Substrings That Can Be Rearranged to Contain a String II

Hard

You are given two strings `word1` and `word2`.

A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.

Return the total number of **valid** substrings of `word1`.

**Note** that the memory limits in this problem are **smaller** than usual, so you **must** implement a solution with a _linear_ runtime complexity.

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

*   <code>1 <= word1.length <= 10<sup>6</sup></code>
*   <code>1 <= word2.length <= 10<sup>4</sup></code>
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun validSubstringCount(word1: String, word2: String): Long {
        val ar = word1.toCharArray()
        val n = ar.size
        val temp = word2.toCharArray()
        val f = IntArray(26)
        for (i in temp) {
            f[i.code - 97]++
        }
        var ans: Long = 0
        var needed = temp.size
        var beg = 0
        var end = 0
        while (end < n) {
            if (f[ar[end].code - 97]-- > 0) {
                needed--
            }
            while (needed == 0) {
                // All substrings from [beg, i], where end <= i < n are valid
                ans += (n - end).toLong()
                // Shrink
                if (f[ar[beg++].code - 97]++ == 0) {
                    needed++
                }
            }
            end++
        }
        return ans
    }
}
```