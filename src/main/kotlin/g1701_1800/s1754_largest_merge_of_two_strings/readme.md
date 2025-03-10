[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1754\. Largest Merge Of Two Strings

Medium

You are given two strings `word1` and `word2`. You want to construct a string `merge` in the following way: while either `word1` or `word2` are non-empty, choose **one** of the following options:

*   If `word1` is non-empty, append the **first** character in `word1` to `merge` and delete it from `word1`.
    *   For example, if `word1 = "abc"` and `merge = "dv"`, then after choosing this operation, `word1 = "bc"` and `merge = "dva"`.
*   If `word2` is non-empty, append the **first** character in `word2` to `merge` and delete it from `word2`.
    *   For example, if `word2 = "abc"` and `merge = ""`, then after choosing this operation, `word2 = "bc"` and `merge = "a"`.

Return _the lexicographically **largest**_ `merge` _you can construct_.

A string `a` is lexicographically larger than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`. For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.

**Example 1:**

**Input:** word1 = "cabaa", word2 = "bcaaa"

**Output:** "cbcabaaaaa"

**Explanation:** One way to get the lexicographically largest merge is: 

- Take from word1: merge = "c", word1 = "abaa", word2 = "bcaaa" 

- Take from word2: merge = "cb", word1 = "abaa", word2 = "caaa" 

- Take from word2: merge = "cbc", word1 = "abaa", word2 = "aaa" 

- Take from word1: merge = "cbca", word1 = "baa", word2 = "aaa" 

- Take from word1: merge = "cbcab", word1 = "aa", word2 = "aaa" 

- Append the remaining 5 a's from word1 and word2 at the end of merge.

**Example 2:**

**Input:** word1 = "abcabc", word2 = "abdcaba"

**Output:** "abdcabcabcaba"

**Constraints:**

*   `1 <= word1.length, word2.length <= 3000`
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun largestMerge(word1: String, word2: String): String {
        val a = word1.toCharArray()
        val b = word2.toCharArray()
        val sb = StringBuilder()
        var i = 0
        var j = 0
        while (i < a.size && j < b.size) {
            if (a[i] == b[j]) {
                val first = go(a, i, b, j)
                if (first) {
                    sb.append(a[i])
                    i++
                } else {
                    sb.append(b[j])
                    j++
                }
            } else {
                if (a[i] > b[j]) {
                    sb.append(a[i])
                    i++
                } else {
                    sb.append(b[j])
                    j++
                }
            }
        }
        while (i < a.size) {
            sb.append(a[i++])
        }
        while (j < b.size) {
            sb.append(b[j++])
        }
        return sb.toString()
    }

    private fun go(a: CharArray, i: Int, b: CharArray, j: Int): Boolean {
        var i = i
        var j = j
        while (i < a.size && j < b.size && a[i] == b[j]) {
            i++
            j++
        }
        if (i == a.size) {
            return false
        }
        return if (j == b.size) {
            true
        } else {
            a[i] > b[j]
        }
    }
}
```