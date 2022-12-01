[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 383\. Ransom Note

Easy

Given two strings `ransomNote` and `magazine`, return `true` _if_ `ransomNote` _can be constructed by using the letters from_ `magazine` _and_ `false` _otherwise_.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

**Input:** ransomNote = "a", magazine = "b"

**Output:** false

**Example 2:**

**Input:** ransomNote = "aa", magazine = "ab"

**Output:** false

**Example 3:**

**Input:** ransomNote = "aa", magazine = "aab"

**Output:** true

**Constraints:**

*   <code>1 <= ransomNote.length, magazine.length <= 10<sup>5</sup></code>
*   `ransomNote` and `magazine` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun canConstruct(ransomNote: String, magazine: String): Boolean {
        val a = IntArray(26)
        var n = ransomNote.length
        for (i in 0 until n) {
            a[ransomNote[i].code - 97]++
        }
        var i = 0
        while (i < magazine.length && n != 0) {
            if (a[magazine[i].code - 97] > 0) {
                n--
                a[magazine[i].code - 97]--
            }
            i++
        }
        return n == 0
    }
}
```