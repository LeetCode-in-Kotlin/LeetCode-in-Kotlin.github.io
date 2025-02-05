[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3442\. Maximum Difference Between Even and Odd Frequency I

Easy

You are given a string `s` consisting of lowercase English letters. Your task is to find the **maximum** difference between the frequency of **two** characters in the string such that:

*   One of the characters has an **even frequency** in the string.
*   The other character has an **odd frequency** in the string.

Return the **maximum** difference, calculated as the frequency of the character with an **odd** frequency **minus** the frequency of the character with an **even** frequency.

**Example 1:**

**Input:** s = "aaaaabbc"

**Output:** 3

**Explanation:**

*   The character `'a'` has an **odd frequency** of `5`, and `'b'` has an **even frequency** of `2`.
*   The maximum difference is `5 - 2 = 3`.

**Example 2:**

**Input:** s = "abcabcab"

**Output:** 1

**Explanation:**

*   The character `'a'` has an **odd frequency** of `3`, and `'c'` has an **even frequency** of 2.
*   The maximum difference is `3 - 2 = 1`.

**Constraints:**

*   `3 <= s.length <= 100`
*   `s` consists only of lowercase English letters.
*   `s` contains at least one character with an odd frequency and one with an even frequency.

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxDifference(s: String): Int {
        val freq = IntArray(26)
        var maxOdd = 0
        var minEven = 1000
        for (i in 0..<s.length) {
            freq[s[i].code - 'a'.code]++
        }
        for (j in freq) {
            if (j != 0 && j % 2 == 0) {
                minEven = min(minEven, j)
            } else {
                maxOdd = max(maxOdd, j)
            }
        }
        return maxOdd - minEven
    }
}
```