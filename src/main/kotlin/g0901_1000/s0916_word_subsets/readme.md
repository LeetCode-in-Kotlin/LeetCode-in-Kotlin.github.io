[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 916\. Word Subsets

Medium

You are given two string arrays `words1` and `words2`.

A string `b` is a **subset** of string `a` if every letter in `b` occurs in `a` including multiplicity.

*   For example, `"wrr"` is a subset of `"warrior"` but is not a subset of `"world"`.

A string `a` from `words1` is **universal** if for every string `b` in `words2`, `b` is a subset of `a`.

Return an array of all the **universal** strings in `words1`. You may return the answer in **any order**.

**Example 1:**

**Input:** words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["e","o"]

**Output:** ["facebook","google","leetcode"]

**Example 2:**

**Input:** words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["l","e"]

**Output:** ["apple","google","leetcode"]

**Constraints:**

*   <code>1 <= words1.length, words2.length <= 10<sup>4</sup></code>
*   `1 <= words1[i].length, words2[i].length <= 10`
*   `words1[i]` and `words2[i]` consist only of lowercase English letters.
*   All the strings of `words1` are **unique**.

## Solution

```kotlin
class Solution {
    fun wordSubsets(words1: Array<String>, words2: Array<String>): List<String> {
        val l1: MutableList<String> = ArrayList()
        val target = IntArray(26)
        for (s1 in words2) {
            val temp = IntArray(26)
            for (ch1 in s1.toCharArray()) {
                temp[ch1.code - 'a'.code]++
                target[ch1.code - 'a'.code] =
                    target[ch1.code - 'a'.code].coerceAtLeast(temp[ch1.code - 'a'.code])
            }
        }
        for (s1 in words1) {
            val count = IntArray(26)
            for (ch1 in s1.toCharArray()) {
                count[ch1.code - 'a'.code]++
            }
            if (checkIt(target, count)) {
                l1.add(s1)
            }
        }
        return l1
    }

    private fun checkIt(target: IntArray, count: IntArray): Boolean {
        for (i in 0..25) {
            if (count[i] < target[i]) {
                return false
            }
        }
        return true
    }
}
```