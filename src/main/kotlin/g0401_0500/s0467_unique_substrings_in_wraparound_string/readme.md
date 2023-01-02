[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 467\. Unique Substrings in Wraparound String

Medium

We define the string `base` to be the infinite wraparound string of `"abcdefghijklmnopqrstuvwxyz"`, so `base` will look like this:

*   `"...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."`.

Given a string `s`, return _the number of **unique non-empty substrings** of_ `s` _are present in_ `base`.

**Example 1:**

**Input:** s = "a"

**Output:** 1

**Explanation:** Only the substring "a" of s is in base.

**Example 2:**

**Input:** s = "cac"

**Output:** 2

**Explanation:** There are two substrings ("a", "c") of s in base.

**Example 3:**

**Input:** s = "zab"

**Output:** 6

**Explanation:** There are six substrings ("z", "a", "b", "za", "ab", and "zab") of s in base.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun findSubstringInWraproundString(p: String): Int {
        val str = p.toCharArray()
        val n = str.size
        val map = IntArray(26)
        var len = 0
        for (i in 0 until n) {
            if (i > 0 && (str[i - 1].code + 1 == str[i].code || str[i - 1] == 'z' && str[i] == 'a')) {
                len += 1
            } else {
                len = 1
            }
            // we are storing the max len of string for each letter and then we will count all these
            // length.
            map[str[i].code - 'a'.code] = Math.max(map[str[i].code - 'a'.code], len)
        }
        var answer = 0
        for (num in map) {
            answer += num
        }
        return answer
    }
}
```