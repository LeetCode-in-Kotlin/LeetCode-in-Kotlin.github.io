[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 567\. Permutation in String

Medium

Given two strings `s1` and `s2`, return `true` _if_ `s2` _contains a permutation of_ `s1`_, or_ `false` _otherwise_.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

**Input:** s1 = "ab", s2 = "eidbaooo"

**Output:** true

**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:** s1 = "ab", s2 = "eidboaoo"

**Output:** false

**Constraints:**

*   <code>1 <= s1.length, s2.length <= 10<sup>4</sup></code>
*   `s1` and `s2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun checkInclusion(s1: String, s2: String): Boolean {
        val n = s1.length
        val m = s2.length
        if (n > m) {
            return false
        }
        val cntS1 = IntArray(26)
        val cntS2 = IntArray(26)
        for (i in 0 until n) {
            cntS1[s1[i].code - 'a'.code]++
        }
        for (i in 0 until n) {
            cntS2[s2[i].code - 'a'.code]++
        }
        if (check(cntS1, cntS2)) {
            return true
        }
        for (i in n until m) {
            cntS2[s2[i - n].code - 'a'.code]--
            cntS2[s2[i].code - 'a'.code]++
            if (check(cntS1, cntS2)) {
                return true
            }
        }
        return false
    }

    private fun check(cnt1: IntArray, cnt2: IntArray): Boolean {
        for (i in 0..25) {
            if (cnt1[i] != cnt2[i]) {
                return false
            }
        }
        return true
    }
}
```