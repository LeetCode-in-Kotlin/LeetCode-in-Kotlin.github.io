[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1247\. Minimum Swaps to Make Strings Equal

Medium

You are given two strings `s1` and `s2` of equal length consisting of letters `"x"` and `"y"` **only**. Your task is to make these two strings equal to each other. You can swap any two characters that belong to **different** strings, which means: swap `s1[i]` and `s2[j]`.

Return the minimum number of swaps required to make `s1` and `s2` equal, or return `-1` if it is impossible to do so.

**Example 1:**

**Input:** s1 = "xx", s2 = "yy"

**Output:** 1

**Explanation:** Swap s1[0] and s2[1], s1 = "yx", s2 = "yx".

**Example 2:**

**Input:** s1 = "xy", s2 = "yx"

**Output:** 2

**Explanation:** 

Swap s1[0] and s2[0], s1 = "yy", s2 = "xx".

Swap s1[0] and s2[1], s1 = "xy", s2 = "xy". 

Note that you cannot swap s1[0] and s1[1] to make s1 equal to "yx", cause we can only swap chars in different strings.

**Example 3:**

**Input:** s1 = "xx", s2 = "xy"

**Output:** -1

**Constraints:**

*   `1 <= s1.length, s2.length <= 1000`
*   `s1, s2` only contain `'x'` or `'y'`.

## Solution

```kotlin
class Solution {
    fun minimumSwap(s1: String, s2: String): Int {
        val len = s1.length
        var countX1 = 0
        var countY1 = 0
        var countX2 = 0
        var countY2 = 0
        for (i in 0 until len) {
            if (s1[i] != s2[i]) {
                if (s1[i] == 'x') {
                    countX1++
                } else {
                    countY1++
                }
                if (s2[i] == 'x') {
                    countX2++
                } else {
                    countY2++
                }
            }
        }
        if ((countX1 + countX2) % 2 == 1) {
            return -1
        }
        return if (countX1 + countX2 > countY1 + countY2) {
            Math.ceil(countY1 * 1.0 / 2).toInt() + Math.ceil(countY2 * 1.0 / 2).toInt()
        } else {
            Math.ceil(countX1 * 1.0 / 2).toInt() + Math.ceil(countX2 * 1.0 / 2).toInt()
        }
    }
}
```