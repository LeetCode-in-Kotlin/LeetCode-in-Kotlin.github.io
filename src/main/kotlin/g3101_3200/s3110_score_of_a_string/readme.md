[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3110\. Score of a String

Easy

You are given a string `s`. The **score** of a string is defined as the sum of the absolute difference between the **ASCII** values of adjacent characters.

Return the **score** of `s`.

**Example 1:**

**Input:** s = "hello"

**Output:** 13

**Explanation:**

The **ASCII** values of the characters in `s` are: `'h' = 104`, `'e' = 101`, `'l' = 108`, `'o' = 111`. So, the score of `s` would be `|104 - 101| + |101 - 108| + |108 - 108| + |108 - 111| = 3 + 7 + 0 + 3 = 13`.

**Example 2:**

**Input:** s = "zaz"

**Output:** 50

**Explanation:**

The **ASCII** values of the characters in `s` are: `'z' = 122`, `'a' = 97`. So, the score of `s` would be `|122 - 97| + |97 - 122| = 25 + 25 = 50`.

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun scoreOfString(s: String): Int {
        var sum = 0
        for (i in 0 until s.length - 1) {
            sum += abs(((s[i].code - '0'.code) - (s[i + 1].code - '0'.code)))
        }
        return sum
    }
}
```