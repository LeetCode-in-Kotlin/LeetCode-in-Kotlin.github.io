[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3541\. Find Most Frequent Vowel and Consonant

Easy

You are given a string `s` consisting of lowercase English letters (`'a'` to `'z'`).

Your task is to:

*   Find the vowel (one of `'a'`, `'e'`, `'i'`, `'o'`, or `'u'`) with the **maximum** frequency.
*   Find the consonant (all other letters excluding vowels) with the **maximum** frequency.

Return the sum of the two frequencies.

**Note**: If multiple vowels or consonants have the same maximum frequency, you may choose any one of them. If there are no vowels or no consonants in the string, consider their frequency as 0.

The **frequency** of a letter `x` is the number of times it occurs in the string.

**Example 1:**

**Input:** s = "successes"

**Output:** 6

**Explanation:**

*   The vowels are: `'u'` (frequency 1), `'e'` (frequency 2). The maximum frequency is 2.
*   The consonants are: `'s'` (frequency 4), `'c'` (frequency 2). The maximum frequency is 4.
*   The output is `2 + 4 = 6`.

**Example 2:**

**Input:** s = "aeiaeia"

**Output:** 3

**Explanation:**

*   The vowels are: `'a'` (frequency 3), `'e'` ( frequency 2), `'i'` (frequency 2). The maximum frequency is 3.
*   There are no consonants in `s`. Hence, maximum consonant frequency = 0.
*   The output is `3 + 0 = 3`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters only.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxFreqSum(s: String): Int {
        val freq = IntArray(26)
        for (ch in s.toCharArray()) {
            val index = ch.code - 'a'.code
            freq[index]++
        }
        val si = "aeiou"
        var max1 = 0
        var max2 = 0
        for (i in 0..25) {
            val ch = (i + 'a'.code).toChar()
            if (si.indexOf(ch) != -1) {
                max1 = max(max1, freq[i])
            } else {
                max2 = max(max2, freq[i])
            }
        }
        return max1 + max2
    }
}
```