[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1967\. Number of Strings That Appear as Substrings in Word

Easy

Given an array of strings `patterns` and a string `word`, return _the **number** of strings in_ `patterns` _that exist as a **substring** in_ `word`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** patterns = ["a","abc","bc","d"], word = "abc"

**Output:** 3

**Explanation:** - "a" appears as a substring in "abc".

- "abc" appears as a substring in "abc".

- "bc" appears as a substring in "abc".

- "d" does not appear as a substring in "abc". 
  
3 of the strings in patterns appear as a substring in word.

**Example 2:**

**Input:** patterns = ["a","b","c"], word = "aaaaabbbbb"

**Output:** 2

**Explanation:** - "a" appears as a substring in "aaaaabbbbb". 

- "b" appears as a substring in "aaaaabbbbb". 

- "c" does not appear as a substring in "aaaaabbbbb". 
  
2 of the strings in patterns appear as a substring in word.

**Example 3:**

**Input:** patterns = ["a","a","a"], word = "ab"

**Output:** 3

**Explanation:** Each of the patterns appears as a substring in word "ab".

**Constraints:**

*   `1 <= patterns.length <= 100`
*   `1 <= patterns[i].length <= 100`
*   `1 <= word.length <= 100`
*   `patterns[i]` and `word` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numOfStrings(patterns: Array<String>, word: String): Int {
        var res = 0
        for (p in patterns) {
            if (word.contains(p)) {
                res++
            }
        }
        return res
    }
}
```