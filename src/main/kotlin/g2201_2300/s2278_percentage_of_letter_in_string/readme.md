[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2278\. Percentage of Letter in String

Easy

Given a string `s` and a character `letter`, return _the **percentage** of characters in_ `s` _that equal_ `letter` _**rounded down** to the nearest whole percent._

**Example 1:**

**Input:** s = "foobar", letter = "o"

**Output:** 33

**Explanation:**

The percentage of characters in s that equal the letter 'o' is 2 / 6 \* 100% = 33% when rounded down, so we return 33.

**Example 2:**

**Input:** s = "jjjj", letter = "k"

**Output:** 0

**Explanation:**

The percentage of characters in s that equal the letter 'k' is 0%, so we return 0.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.
*   `letter` is a lowercase English letter.

## Solution

```kotlin
class Solution {
    fun percentageLetter(s: String, letter: Char): Int {
        var count = 0
        val n = s.length
        for (i in 0 until n) {
            if (s[i] == letter) {
                ++count
            }
        }
        return count * 100 / n
    }
}
```