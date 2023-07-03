[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2108\. Find First Palindromic String in the Array

Easy

Given an array of strings `words`, return _the first **palindromic** string in the array_. If there is no such string, return _an **empty string**_ `""`.

A string is **palindromic** if it reads the same forward and backward.

**Example 1:**

**Input:** words = ["abc","car","ada","racecar","cool"]

**Output:** "ada"

**Explanation:** The first string that is palindromic is "ada". Note that "racecar" is also palindromic, but it is not the first.

**Example 2:**

**Input:** words = ["notapalindrome","racecar"]

**Output:** "racecar"

**Explanation:** The first and only string that is palindromic is "racecar".

**Example 3:**

**Input:** words = ["def","ghi"]

**Output:** ""

**Explanation:** There are no palindromic strings, so the empty string is returned.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun firstPalindrome(words: Array<String>): String {
        for (word in words) {
            if (isPalindrome(word)) {
                return word
            }
        }
        return ""
    }

    companion object {
        fun isPalindrome(s: String): Boolean {
            val len = s.length
            var i = 0
            var j = len - 1
            while (i <= len / 2 && j >= len / 2) {
                if (s[i] != s[j]) {
                    return false
                }
                i++
                j--
            }
            return true
        }
    }
}
```