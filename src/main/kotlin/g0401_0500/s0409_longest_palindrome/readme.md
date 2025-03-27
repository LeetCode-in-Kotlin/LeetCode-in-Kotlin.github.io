[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 409\. Longest Palindrome

Easy

Given a string `s` which consists of lowercase or uppercase letters, return _the length of the **longest palindrome**_ that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

**Example 1:**

**Input:** s = "abccccdd"

**Output:** 7

**Explanation:** One longest palindrome that can be built is "dccaccd", whose length is 7.

**Example 2:**

**Input:** s = "a"

**Output:** 1

**Explanation:** The longest palindrome that can be built is "a", whose length is 1.

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lowercase **and/or** uppercase English letters only.

## Solution

```kotlin
import java.util.BitSet

class Solution {
    fun longestPalindrome(s: String): Int {
        val set = BitSet(60)
        for (c in s.toCharArray()) {
            set.flip(c.code - 'A'.code)
        }
        return if (set.isEmpty) {
            s.length
        } else {
            s.length - set.cardinality() + 1
        }
    }
}
```