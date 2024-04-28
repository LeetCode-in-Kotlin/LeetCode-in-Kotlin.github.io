[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3121\. Count the Number of Special Characters II

Medium

You are given a string `word`. A letter `c` is called **special** if it appears **both** in lowercase and uppercase in `word`, and **every** lowercase occurrence of `c` appears before the **first** uppercase occurrence of `c`.

Return the number of **special** letters in `word`.

**Example 1:**

**Input:** word = "aaAbcBC"

**Output:** 3

**Explanation:**

The special characters are `'a'`, `'b'`, and `'c'`.

**Example 2:**

**Input:** word = "abc"

**Output:** 0

**Explanation:**

There are no special characters in `word`.

**Example 3:**

**Input:** word = "AbBCab"

**Output:** 0

**Explanation:**

There are no special characters in `word`.

**Constraints:**

*   <code>1 <= word.length <= 2 * 10<sup>5</sup></code>
*   `word` consists of only lowercase and uppercase English letters.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun numberOfSpecialChars(word: String): Int {
        val small = IntArray(26)
        small.fill(-1)
        val capital = IntArray(26)
        capital.fill(Int.MAX_VALUE)
        var result = 0
        for (i in word.indices) {
            val a = word[i]
            if (a.code < 91) {
                capital[a.code - 65] = min(capital[a.code - 65].toDouble(), i.toDouble()).toInt()
            } else {
                small[a.code - 97] = i
            }
        }
        for (i in 0..25) {
            if (-1 != small[i] && Int.MAX_VALUE != capital[i] && capital[i] > small[i]) {
                result++
            }
        }
        return result
    }
}
```