[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2953\. Count Complete Substrings

Hard

You are given a string `word` and an integer `k`.

A substring `s` of `word` is **complete** if:

*   Each character in `s` occurs **exactly** `k` times.
*   The difference between two adjacent characters is **at most** `2`. That is, for any two adjacent characters `c1` and `c2` in `s`, the absolute difference in their positions in the alphabet is **at most** `2`.

Return _the number of **complete** substrings of_ `word`.

A **substring** is a **non-empty** contiguous sequence of characters in a string.

**Example 1:**

**Input:** word = "igigee", k = 2

**Output:** 3

**Explanation:** The complete substrings where each character appears exactly twice and the difference between adjacent characters is at most 2 are: <ins>**igig**</ins>ee, igig<ins>**ee**</ins>, <ins>**igigee**</ins>.

**Example 2:**

**Input:** word = "aaabbbccc", k = 3

**Output:** 6

**Explanation:** The complete substrings where each character appears exactly three times and the difference between adjacent characters is at most 2 are: **<ins>aaa</ins>**bbbccc, aaa<ins>**bbb**</ins>ccc, aaabbb<ins>**ccc**</ins>, **<ins>aaabbb</ins>**ccc, aaa<ins>**bbbccc**</ins>, <ins>**aaabbbccc**</ins>.

**Constraints:**

*   <code>1 <= word.length <= 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `1 <= k <= word.length`

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun countCompleteSubstrings(word: String, k: Int): Int {
        val arr = word.toCharArray()
        val n = arr.size
        var result = 0
        var last = 0
        for (i in 1..n) {
            if (i == n || abs((arr[i].code - arr[i - 1].code).toDouble()) > 2) {
                result += getCount(arr, k, last, i - 1)
                last = i
            }
        }
        return result
    }

    private fun getCount(arr: CharArray, k: Int, start: Int, end: Int): Int {
        var result = 0
        var i = 1
        while (i <= 26 && i * k <= end - start + 1) {
            val cnt = IntArray(26)
            var good = 0
            for (j in start..end) {
                val cR = arr[j]
                cnt[cR.code - 'a'.code]++
                if (cnt[cR.code - 'a'.code] == k) {
                    good++
                }
                if (cnt[cR.code - 'a'.code] == k + 1) {
                    good--
                }
                if (j >= start + i * k) {
                    val cL = arr[j - i * k]
                    if (cnt[cL.code - 'a'.code] == k) {
                        good--
                    }
                    if (cnt[cL.code - 'a'.code] == k + 1) {
                        good++
                    }
                    cnt[cL.code - 'a'.code]--
                }
                if (good == i) {
                    result++
                }
            }
            i++
        }
        return result
    }
}
```