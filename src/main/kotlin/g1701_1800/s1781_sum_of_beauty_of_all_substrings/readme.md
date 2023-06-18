[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1781\. Sum of Beauty of All Substrings

Medium

The **beauty** of a string is the difference in frequencies between the most frequent and least frequent characters.

*   For example, the beauty of `"abaacc"` is `3 - 1 = 2`.

Given a string `s`, return _the sum of **beauty** of all of its substrings._

**Example 1:**

**Input:** s = "aabcb"

**Output:** 5

**Explanation:** The substrings with non-zero beauty are ["aab","aabc","aabcb","abcb","bcb"], each with beauty equal to 1.

**Example 2:**

**Input:** s = "aabcbaa"

**Output:** 17 

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun beautySum(s: String): Int {
        var beauty = 0
        for (i in s.indices) {
            val numCountOfFreq = IntArray(s.length + 1 - i)
            val charFreq = IntArray(26)
            charFreq[s[i].code - 'a'.code] = 1
            numCountOfFreq[1] = 1
            var min = 1
            var max = 1
            for (j in i + 1 until s.length) {
                val c = s[j]
                charFreq[c.code - 'a'.code]++
                val freq = charFreq[c.code - 'a'.code]
                numCountOfFreq[freq - 1]--
                numCountOfFreq[freq]++
                if (numCountOfFreq[min] == 0) {
                    min++
                }
                if (min > freq) {
                    min = freq
                }
                if (max < freq) {
                    max = freq
                }
                beauty += max - min
            }
        }
        return beauty
    }
}
```