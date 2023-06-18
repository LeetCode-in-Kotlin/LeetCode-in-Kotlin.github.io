[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1684\. Count the Number of Consistent Strings

Easy

You are given a string `allowed` consisting of **distinct** characters and an array of strings `words`. A string is **consistent** if all characters in the string appear in the string `allowed`.

Return _the number of **consistent** strings in the array_ `words`.

**Example 1:**

**Input:** allowed = "ab", words = ["ad","bd","aaab","baa","badab"]

**Output:** 2

**Explanation:** Strings "aaab" and "baa" are consistent since they only contain characters 'a' and 'b'.

**Example 2:**

**Input:** allowed = "abc", words = ["a","b","c","ab","ac","bc","abc"]

**Output:** 7

**Explanation:** All strings are consistent.

**Example 3:**

**Input:** allowed = "cad", words = ["cc","acd","b","ba","bac","bad","ac","d"]

**Output:** 4

**Explanation:** Strings "cc", "acd", "ac", and "d" are consistent.

**Constraints:**

*   <code>1 <= words.length <= 10<sup>4</sup></code>
*   `1 <= allowed.length <= 26`
*   `1 <= words[i].length <= 10`
*   The characters in `allowed` are **distinct**.
*   `words[i]` and `allowed` contain only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun countConsistentStrings(allowed: String, words: Array<String>): Int {
        var alwd = 0
        var res = 0
        for (i in 0 until allowed.length) {
            alwd = alwd or (1 shl allowed[i].code)
        }
        for (word in words) {
            var b = 0
            for (j in 0 until word.length) {
                b = b or (1 shl word[j].code)
            }
            if (alwd or b == alwd) {
                ++res
            }
        }
        return res
    }
}
```