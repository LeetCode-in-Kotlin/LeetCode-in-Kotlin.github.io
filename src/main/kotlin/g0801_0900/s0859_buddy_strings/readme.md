[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 859\. Buddy Strings

Easy

Given two strings `s` and `goal`, return `true` _if you can swap two letters in_ `s` _so the result is equal to_ `goal`_, otherwise, return_ `false`_._

Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such that `i != j` and swapping the characters at `s[i]` and `s[j]`.

*   For example, swapping at indices `0` and `2` in `"abcd"` results in `"cbad"`.

**Example 1:**

**Input:** s = "ab", goal = "ba"

**Output:** true

**Explanation:** You can swap s[0] = 'a' and s[1] = 'b' to get "ba", which is equal to goal.

**Example 2:**

**Input:** s = "ab", goal = "ab"

**Output:** false

**Explanation:** The only letters you can swap are s[0] = 'a' and s[1] = 'b', which results in "ba" != goal.

**Example 3:**

**Input:** s = "aa", goal = "aa"

**Output:** true

**Explanation:** You can swap s[0] = 'a' and s[1] = 'a' to get "aa", which is equal to goal.

**Constraints:**

*   <code>1 <= s.length, goal.length <= 2 * 10<sup>4</sup></code>
*   `s` and `goal` consist of lowercase letters.

## Solution

```kotlin
class Solution {
    fun buddyStrings(s: String, goal: String): Boolean {
        var first = -1
        val second: Int
        val sCounts = IntArray(26)
        if (s == goal) {
            for (i in s.indices) {
                sCounts[s[i].code - 'a'.code]++
                if (sCounts[s[i].code - 'a'.code] > 1) {
                    return true
                }
            }
        }
        for (i in s.indices) {
            val curr = s[i]
            sCounts[curr.code - 'a'.code]++
            if (curr != goal[i]) {
                if (first == -1) {
                    first = i
                } else {
                    second = i
                    val ss = s.toCharArray()
                    val temp = ss[first]
                    ss[first] = ss[second]
                    ss[second] = temp
                    return String(ss) == goal
                }
            }
        }
        return false
    }
}
```