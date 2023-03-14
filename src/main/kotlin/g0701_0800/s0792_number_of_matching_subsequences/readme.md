[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 792\. Number of Matching Subsequences

Medium

Given a string `s` and an array of strings `words`, return _the number of_ `words[i]` _that is a subsequence of_ `s`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

*   For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**

**Input:** s = "abcde", words = ["a","bb","acd","ace"]

**Output:** 3

**Explanation:** There are three strings in words that are a subsequence of s: "a", "acd", "ace".

**Example 2:**

**Input:** s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]

**Output:** 2

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 50`
*   `s` and `words[i]` consist of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numMatchingSubseq(s: String, words: Array<String>): Int {
        val buckets: Array<MutableList<Node>?> = arrayOfNulls(26)
        for (i in buckets.indices) {
            buckets[i] = ArrayList()
        }
        for (word in words) {
            val start = word[0]
            buckets[start.code - 'a'.code]?.add(Node(word, 0))
        }
        var result = 0
        for (c in s.toCharArray()) {
            val currBucket: MutableList<Node>? = buckets[c.code - 'a'.code]
            buckets[c.code - 'a'.code] = ArrayList()
            for (node in currBucket!!) {
                node.index++
                if (node.index == node.word.length) {
                    result++
                } else {
                    val start = node.word[node.index]
                    buckets[start.code - 'a'.code]?.add(node)
                }
            }
        }
        return result
    }

    private class Node(var word: String, var index: Int)
}
```