[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2351\. First Letter to Appear Twice

Easy

Given a string `s` consisting of lowercase English letters, return _the first letter to appear **twice**_.

**Note**:

*   A letter `a` appears twice before another letter `b` if the **second** occurrence of `a` is before the **second** occurrence of `b`.
*   `s` will contain at least one letter that appears twice.

**Example 1:**

**Input:** s = "abccbaacz"

**Output:** "c"

**Explanation:**

The letter 'a' appears on the indexes 0, 5 and 6.

The letter 'b' appears on the indexes 1 and 4.

The letter 'c' appears on the indexes 2, 3 and 7.

The letter 'z' appears on the index 8.

The letter 'c' is the first letter to appear twice, because out of all the letters the index of its second occurrence is the smallest.

**Example 2:**

**Input:** s = "abcdd"

**Output:** "d"

**Explanation:**

The only letter that appears twice is 'd' so we return 'd'. 

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists of lowercase English letters.
*   `s` has at least one repeated letter.

## Solution

```kotlin
class Solution {
    fun repeatedCharacter(s: String): Char {
        val n = s.length
        val hm = IntArray(26)
        for (i in 0 until n) {
            val c = s[i]
            hm[c.code - 'a'.code]++
            if (hm[c.code - 'a'.code] > 1) {
                return c
            }
        }
        return '0'
    }
}
```