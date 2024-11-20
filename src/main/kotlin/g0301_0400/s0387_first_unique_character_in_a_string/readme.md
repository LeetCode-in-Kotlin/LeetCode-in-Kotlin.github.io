[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 387\. First Unique Character in a String

Easy

Given a string `s`, _find the first non-repeating character in it and return its index_. If it does not exist, return `-1`.

**Example 1:**

**Input:** s = "leetcode"

**Output:** 0

**Example 2:**

**Input:** s = "loveleetcode"

**Output:** 2

**Example 3:**

**Input:** s = "aabb"

**Output:** -1

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun firstUniqChar(s: String): Int {
        var ans = Int.MAX_VALUE
        var i = 'a'
        while (i <= 'z') {
            val ind = s.indexOf(i)
            if (ind != -1 && ind == s.lastIndexOf(i)) {
                ans = Math.min(ans, ind)
            }
            i++
        }
        return if (ans == Int.MAX_VALUE) {
            -1
        } else {
            ans
        }
    }
}
```