[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 940\. Distinct Subsequences II

Hard

Given a string s, return _the number of **distinct non-empty subsequences** of_ `s`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of <code>"<ins>a</ins>b<ins>c</ins>d<ins>e</ins>"</code> while `"aec"` is not.

**Example 1:**

**Input:** s = "abc"

**Output:** 7

**Explanation:** The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".

**Example 2:**

**Input:** s = "aba"

**Output:** 6

**Explanation:** The 6 distinct subsequences are "a", "b", "ab", "aa", "ba", and "aba".

**Example 3:**

**Input:** s = "aaa"

**Output:** 3

**Explanation:** The 3 distinct subsequences are "a", "aa" and "aaa".

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun distinctSubseqII(s: String): Int {
        val n = s.length
        val mod = 1000000007
        val arr = IntArray(26)
        for (i in 0 until n) {
            val x = s[i].code - 'a'.code
            var sum: Long = 0
            arr[x] += 1
            for (j in 0..25) {
                sum = (sum + arr[j]) % mod
            }
            arr[x] = sum.toInt()
        }
        var total: Long = 0
        for (x in arr) {
            total = (total + x) % mod
        }
        return total.toInt()
    }
}
```