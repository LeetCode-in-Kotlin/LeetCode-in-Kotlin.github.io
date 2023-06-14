[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1297\. Maximum Number of Occurrences of a Substring

Medium

Given a string `s`, return the maximum number of ocurrences of **any** substring under the following rules:

*   The number of unique characters in the substring must be less than or equal to `maxLetters`.
*   The substring size must be between `minSize` and `maxSize` inclusive.

**Example 1:**

**Input:** s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4

**Output:** 2

**Explanation:** Substring "aab" has 2 ocurrences in the original string.

It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).

**Example 2:**

**Input:** s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3

**Output:** 2

**Explanation:** Substring "aaa" occur 2 times in the string. It can overlap.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `1 <= maxLetters <= 26`
*   `1 <= minSize <= maxSize <= min(26, s.length)`
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
@Suppress("UNUSED_PARAMETER")
class Solution {
    fun maxFreq(s: String, max: Int, minSize: Int, maxSize: Int): Int {
        // the map of occurrences
        val sub2Count: MutableMap<String, Int> = HashMap()
        // sliding window indices
        var lo = 0
        var hi = minSize - 1
        var maxCount = 0
        // unique letters counter
        val uniq = CharArray(26)
        var uniqCount = 0
        // initial window calculation - `hi` is excluded here!
        for (ch in s.substring(lo, hi).toCharArray()) {
            uniq[ch.code - 'a'.code] = uniq[ch.code - 'a'.code] + 1.toChar().code
            if (uniq[ch.code - 'a'.code].code == 1) {
                uniqCount++
            }
        }
        while (hi < s.length) {
            // handle increment of hi
            val hiCh = s[hi]
            uniq[hiCh.code - 'a'.code] = uniq[hiCh.code - 'a'.code] + 1.toChar().code
            if (uniq[hiCh.code - 'a'.code].code == 1) {
                uniqCount++
            }
            ++hi
            // add the substring to the map of occurences
            val sub = s.substring(lo, hi)
            if (uniqCount <= max) {
                var count = 1
                if (sub2Count.containsKey(sub)) {
                    count += sub2Count[sub]!!
                }
                sub2Count[sub] = count
                maxCount = Math.max(maxCount, count)
            }
            // handle increment of lo
            val loCh = s[lo]
            uniq[loCh.code - 'a'.code] = uniq[loCh.code - 'a'.code] - 1.toChar().code
            if (uniq[loCh.code - 'a'.code].code == 0) {
                uniqCount--
            }
            ++lo
        }
        return maxCount
    }
}
```