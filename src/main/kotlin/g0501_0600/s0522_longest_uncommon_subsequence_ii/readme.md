[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 522\. Longest Uncommon Subsequence II

Medium

Given an array of strings `strs`, return _the length of the **longest uncommon subsequence** between them_. If the longest uncommon subsequence does not exist, return `-1`.

An **uncommon subsequence** between an array of strings is a string that is a **subsequence of one string but not the others**.

A **subsequence** of a string `s` is a string that can be obtained after deleting any number of characters from `s`.

*   For example, `"abc"` is a subsequence of `"aebdc"` because you can delete the underlined characters in <code>"a<ins>e</ins>b<ins>d</ins>c"</code> to get `"abc"`. Other subsequences of `"aebdc"` include `"aebdc"`, `"aeb"`, and `""` (empty string).

**Example 1:**

**Input:** strs = ["aba","cdc","eae"]

**Output:** 3

**Example 2:**

**Input:** strs = ["aaa","aaa","aa"]

**Output:** -1

**Constraints:**

*   `2 <= strs.length <= 50`
*   `1 <= strs[i].length <= 10`
*   `strs[i]` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findLUSlength(strs: Array<String>): Int {
        var maxUncommonLen = -1
        for (indx1 in strs.indices) {
            var isCommon = false
            for (indx2 in strs.indices) {
                if (indx2 != indx1 && isSubSequence(strs[indx1], strs[indx2])) {
                    isCommon = true
                    break
                }
            }
            if (!isCommon) {
                maxUncommonLen = Math.max(maxUncommonLen, strs[indx1].length)
            }
        }
        return maxUncommonLen
    }

    private fun isSubSequence(str1: String, str2: String): Boolean {
        var indx1 = 0
        for (indx2 in 0 until str2.length) {
            if (str1[indx1] == str2[indx2]) {
                indx1++
            }
            if (indx1 == str1.length) {
                return true
            }
        }
        return indx1 == str1.length
    }
}
```