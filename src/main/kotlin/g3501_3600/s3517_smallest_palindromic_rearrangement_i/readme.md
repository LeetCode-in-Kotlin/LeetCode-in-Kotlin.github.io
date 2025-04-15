[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3517\. Smallest Palindromic Rearrangement I

Medium

You are given a **palindromic** string `s`.

Return the **lexicographically smallest** palindromic permutation of `s`.

**Example 1:**

**Input:** s = "z"

**Output:** "z"

**Explanation:**

A string of only one character is already the lexicographically smallest palindrome.

**Example 2:**

**Input:** s = "babab"

**Output:** "abbba"

**Explanation:**

Rearranging `"babab"` → `"abbba"` gives the smallest lexicographic palindrome.

**Example 3:**

**Input:** s = "daccad"

**Output:** "acddca"

**Explanation:**

Rearranging `"daccad"` → `"acddca"` gives the smallest lexicographic palindrome.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.
*   `s` is guaranteed to be palindromic.

## Solution

```kotlin
class Solution {
    fun smallestPalindrome(s: String): String {
        val n = s.length
        val m = n / 2
        if (n == 1 || n == 2) {
            return s
        }
        val fArr = s.substring(0, m).toCharArray()
        fArr.sort()
        var f = String(fArr)
        val rev = StringBuilder(f).reverse()
        if (n % 2 == 1) {
            f += s[m]
        }
        return f + rev
    }
}
```