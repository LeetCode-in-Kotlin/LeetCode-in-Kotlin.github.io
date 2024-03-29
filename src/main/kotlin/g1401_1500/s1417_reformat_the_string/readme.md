[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1417\. Reformat The String

Easy

You are given an alphanumeric string `s`. (**Alphanumeric string** is a string consisting of lowercase English letters and digits).

You have to find a permutation of the string where no letter is followed by another letter and no digit is followed by another digit. That is, no two adjacent characters have the same type.

Return _the reformatted string_ or return **an empty string** if it is impossible to reformat the string.

**Example 1:**

**Input:** s = "a0b1c2"

**Output:** "0a1b2c"

**Explanation:** No two adjacent characters have the same type in "0a1b2c". "a0b1c2", "0a1b2c", "0c2a1b" are also valid permutations.

**Example 2:**

**Input:** s = "leetcode"

**Output:** ""

**Explanation:** "leetcode" has only characters so we cannot separate them by digits.

**Example 3:**

**Input:** s = "1229857369"

**Output:** ""

**Explanation:** "1229857369" has only digits so we cannot separate them by characters.

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of only lowercase English letters and/or digits.

## Solution

```kotlin
class Solution {
    fun reformat(s: String): String {
        val chars: MutableList<Char> = ArrayList()
        val digits: MutableList<Char> = ArrayList()
        for (c in s.toCharArray()) {
            if (c in '0'..'9') {
                digits.add(c)
            } else {
                chars.add(c)
            }
        }
        if (Math.abs(digits.size - chars.size) > 1) {
            return ""
        }
        var isDigit = digits.size > chars.size
        val sb = StringBuilder()
        for (i in s.indices) {
            if (isDigit) {
                sb.append(digits.removeAt(0))
            } else {
                sb.append(chars.removeAt(0))
            }
            isDigit = !isDigit
        }
        return sb.toString()
    }
}
```