[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1221\. Split a String in Balanced Strings

Easy

**Balanced** strings are those that have an equal quantity of `'L'` and `'R'` characters.

Given a **balanced** string `s`, split it in the maximum amount of balanced strings.

Return _the maximum amount of split **balanced** strings_.

**Example 1:**

**Input:** s = "RLRRLLRLRL"

**Output:** 4

**Explanation:** s can be split into "RL", "RRLL", "RL", "RL", each substring contains same number of 'L' and 'R'.

**Example 2:**

**Input:** s = "RLLLLRRRLR"

**Output:** 3

**Explanation:** s can be split into "RL", "LLLRRR", "LR", each substring contains same number of 'L' and 'R'.

**Example 3:**

**Input:** s = "LLLLRRRR"

**Output:** 1

**Explanation:** s can be split into "LLLLRRRR".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'L'` or `'R'`.
*   `s` is a **balanced** string.

## Solution

```kotlin
class Solution {
    fun balancedStringSplit(s: String): Int {
        var i = 0
        var balancedCount = 0
        var lCount = 0
        var rCount = 0
        while (i < s.length) {
            if (s[i] == 'L') {
                lCount++
            } else {
                rCount++
            }
            i++
            if (lCount != 0 && lCount == rCount) {
                lCount = 0
                rCount = 0
                balancedCount++
            }
        }
        return balancedCount
    }
}
```