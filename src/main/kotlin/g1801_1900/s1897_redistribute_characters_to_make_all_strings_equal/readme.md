[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1897\. Redistribute Characters to Make All Strings Equal

Easy

You are given an array of strings `words` (**0-indexed**).

In one operation, pick two **distinct** indices `i` and `j`, where `words[i]` is a non-empty string, and move **any** character from `words[i]` to **any** position in `words[j]`.

Return `true` _if you can make **every** string in_ `words` _**equal** using **any** number of operations_, _and_ `false` _otherwise_.

**Example 1:**

**Input:** words = ["abc","aabc","bc"]

**Output:** true

**Explanation:** Move the first 'a' in `words[1] to the front of words[2], to make` `words[1]` = "abc" and words[2] = "abc".

All the strings are now equal to "abc", so return `true`.

**Example 2:**

**Input:** words = ["ab","a"]

**Output:** false

**Explanation:** It is impossible to make all the strings equal using the operation.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun makeEqual(words: Array<String>): Boolean {
        val charFreq = IntArray(26)
        for (word in words) {
            for (chIndex in 0 until word.length) {
                charFreq[word[chIndex].code - 'a'.code]++
            }
        }
        for (freq in charFreq) {
            if (freq % words.size != 0) {
                return false
            }
        }
        return true
    }
}
```