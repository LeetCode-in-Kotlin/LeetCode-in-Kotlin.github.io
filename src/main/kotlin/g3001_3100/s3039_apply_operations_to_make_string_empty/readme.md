[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3039\. Apply Operations to Make String Empty

Medium

You are given a string `s`.

Consider performing the following operation until `s` becomes **empty**:

*   For **every** alphabet character from `'a'` to `'z'`, remove the **first** occurrence of that character in `s` (if it exists).

For example, let initially `s = "aabcbbca"`. We do the following operations:

*   Remove the underlined characters <code>s = "<ins>**a**</ins>a**<ins>bc</ins>**bbca"</code>. The resulting string is `s = "abbca"`.
*   Remove the underlined characters <code>s = "<ins>**ab**</ins>b<ins>**c**</ins>a"</code>. The resulting string is `s = "ba"`.
*   Remove the underlined characters <code>s = "<ins>**ba**</ins>"</code>. The resulting string is `s = ""`.

Return _the value of the string_ `s` _right **before** applying the **last** operation_. In the example above, answer is `"ba"`.

**Example 1:**

**Input:** s = "aabcbbca"

**Output:** "ba"

**Explanation:** Explained in the statement.

**Example 2:**

**Input:** s = "abcd"

**Output:** "abcd"

**Explanation:** We do the following operation: 
- Remove the underlined characters s = "<ins>**abcd**</ins>". The resulting string is s = "". 

The string just before the last operation is "abcd".

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun lastNonEmptyString(s: String): String {
        val freq = IntArray(26)
        val ar = s.toCharArray()
        val n = ar.size
        var max = 1
        val sb = StringBuilder()
        for (c in ar) {
            freq[c.code - 'a'.code]++
            max = max(freq[c.code - 'a'.code].toDouble(), max.toDouble()).toInt()
        }
        for (i in n - 1 downTo 0) {
            if (freq[ar[i].code - 'a'.code] == max) {
                sb.append(ar[i])
                freq[ar[i].code - 'a'.code] = 0
            }
        }
        return sb.reverse().toString()
    }
}
```