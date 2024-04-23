[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3090\. Maximum Length Substring With Two Occurrences

Easy

Given a string `s`, return the **maximum** length of a substring such that it contains _at most two occurrences_ of each character.

**Example 1:**

**Input:** s = "bcbbbcba"

**Output:** 4

**Explanation:**

The following substring has a length of 4 and contains at most two occurrences of each character: <code>"bcbb<ins>bcba</ins>"</code>.

**Example 2:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:**

The following substring has a length of 2 and contains at most two occurrences of each character: <code>"<ins>aa</ins>aa"</code>.

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLengthSubstring(s: String): Int {
        val freq = IntArray(26)
        val chars = s.toCharArray()
        var i = 0
        val len = s.length
        var max = 0
        for (j in 0 until len) {
            ++freq[chars[j].code - 'a'.code]
            while (freq[chars[j].code - 'a'.code] == 3) {
                --freq[chars[i].code - 'a'.code]
                i++
            }
            max = max(max, (j - i + 1))
        }
        return max
    }
}
```