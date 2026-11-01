[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3844\. Longest Almost-Palindromic Substring

Medium

You are given a string `s` consisting of lowercase English letters.

A substring is **almost-palindromic** if it becomes a palindrome after removing **exactly** one character from it.

Return an integer denoting the length of the **longest** **almost-palindromic** substring in `s`.

**Example 1:**

**Input:** s = "abca"

**Output:** 4

**Explanation:**

Choose the substring <code>"<ins>**abca**</ins>"</code>.

*   Remove <code>"ab<ins>**c**</ins>a"</code>.
*   The string becomes `"aba"`, which is a palindrome.
*   Therefore, `"abca"` is almost-palindromic.

**Example 2:**

**Input:** s = "abba"

**Output:** 4

**Explanation:**

Choose the substring <code>"<ins>**abba**</ins>"</code>.

*   Remove <code>"a<ins>**b**</ins>ba"</code>.
*   The string becomes `"aba"`, which is a palindrome.
*   Therefore, `"abba"` is almost-palindromic.

**Example 3:**

**Input:** s = "zzabba"

**Output:** 5

**Explanation:**

Choose the substring <code>"z<ins>**zabba**</ins>"</code>.

*   Remove <code>"<ins>**z**</ins>abba"</code>.
*   The string becomes `"abba"`, which is a palindrome.
*   Therefore, `"zabba"` is almost-palindromic.

**Constraints:**

*   `2 <= s.length <= 2500`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    private var n = 0
    private lateinit var s: CharArray

    fun almostPalindromic(str: String): Int {
        s = str.toCharArray()
        n = s.size
        var ans = 0
        for (i in 0 until n) {
            ans = maxOf(ans, f(i, i))
            ans = maxOf(ans, f(i, i + 1))
        }
        return ans
    }

    private fun f(l: Int, r: Int): Int {
        var currL = l
        var currR = r
        while (currL >= 0 && currR < n && s[currL] == s[currR]) {
            currL--
            currR++
        }
        var l1 = currL - 1
        var r1 = currR
        var l2 = currL
        var r2 = currR + 1
        while (l1 >= 0 && r1 < n && s[l1] == s[r1]) {
            l1--
            r1++
        }
        while (l2 >= 0 && r2 < n && s[l2] == s[r2]) {
            l2--
            r2++
        }
        return maxOf(r1 - l1 - 1, r2 - l2 - 1).coerceIn(0, n)
    }
}
```