[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2124\. Check if All A's Appears Before All B's

Easy

Given a string `s` consisting of **only** the characters `'a'` and `'b'`, return `true` _if **every**_ `'a'` _appears before **every**_ `'b'` _in the string_. Otherwise, return `false`.

**Example 1:**

**Input:** s = "aaabbb"

**Output:** true

**Explanation:** 

The 'a's are at indices 0, 1, and 2, while the 'b's are at indices 3, 4, and 5. 

Hence, every 'a' appears before every 'b' and we return true.

**Example 2:**

**Input:** s = "abab"

**Output:** false

**Explanation:** 

There is an 'a' at index 2 and a 'b' at index 1. 

Hence, not every 'a' appears before every 'b' and we return false.

**Example 3:**

**Input:** s = "bbb"

**Output:** true

**Explanation:** There are no 'a's, hence, every 'a' appears before every 'b' and we return true.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s[i]` is either `'a'` or `'b'`.

## Solution

```kotlin
class Solution {
    fun checkString(s: String): Boolean {
        var aEndIndex = -1
        var bStartIndex = -1
        if (s.length == 1) {
            return true
        }
        for (i in s.length - 1 downTo 0) {
            if (s[i] == 'a') {
                aEndIndex = i
                break
            }
        }
        for (i in 0..s.length - 1) {
            if (s[i] == 'b') {
                bStartIndex = i
                break
            }
        }
        return if (aEndIndex == -1 || bStartIndex == -1) {
            true
        } else {
            bStartIndex > aEndIndex
        }
    }
}
```