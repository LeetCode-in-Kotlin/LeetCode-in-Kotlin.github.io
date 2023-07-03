[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2272\. Substring With Largest Variance

Hard

The **variance** of a string is defined as the largest difference between the number of occurrences of **any** `2` characters present in the string. Note the two characters may or may not be the same.

Given a string `s` consisting of lowercase English letters only, return _the **largest variance** possible among all **substrings** of_ `s`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "aababbb"

**Output:** 3

**Explanation:** All possible variances along with their respective substrings are listed below: 

- Variance 0 for substrings "a", "aa", "ab", "abab", "aababb", "ba", "b", "bb", and "bbb". 

- Variance 1 for substrings "aab", "aba", "abb", "aabab", "ababb", "aababbb", and "bab". 

- Variance 2 for substrings "aaba", "ababbb", "abbb", and "babb". 

- Variance 3 for substring "babbb". 
  
Since the largest possible variance is 3, we return it.

**Example 2:**

**Input:** s = "abcde"

**Output:** 0

**Explanation:** No letter occurs more than once in s, so the variance of every substring is 0.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun largestVariance(s: String): Int {
        val freq = IntArray(26)
        for (i in 0 until s.length) {
            freq[s[i].code - 'a'.code]++
        }
        var maxVariance = 0
        for (a in 0..25) {
            for (b in 0..25) {
                var remainingA = freq[a]
                val remainingB = freq[b]
                if (a == b || remainingA == 0 || remainingB == 0) {
                    continue
                }
                var currBFreq = 0
                var currAFreq = 0
                for (i in 0 until s.length) {
                    val c = s[i].code - 'a'.code
                    if (c == b) {
                        currBFreq++
                    }
                    if (c == a) {
                        currAFreq++
                        remainingA--
                    }
                    if (currAFreq > 0) {
                        maxVariance = Math.max(maxVariance, currBFreq - currAFreq)
                    }
                    if (currBFreq < currAFreq && remainingA >= 1) {
                        currBFreq = 0
                        currAFreq = 0
                    }
                }
            }
        }
        return maxVariance
    }
}
```