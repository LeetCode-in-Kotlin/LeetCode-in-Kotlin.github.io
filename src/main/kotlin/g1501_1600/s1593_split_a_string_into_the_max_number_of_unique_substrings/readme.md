[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1593\. Split a String Into the Max Number of Unique Substrings

Medium

Given a string `s`, return _the maximum number of unique substrings that the given string can be split into_.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "ababccc"

**Output:** 5

**Explanation:** One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.

**Example 2:**

**Input:** s = "aba"

**Output:** 2

**Explanation:** One way to split maximally is ['a', 'ba'].

**Example 3:**

**Input:** s = "aa"

**Output:** 1

**Explanation:** It is impossible to split the string any further.

**Constraints:**

*   `1 <= s.length <= 16`

*   `s` contains only lower case English letters.

## Solution

```kotlin
class Solution {
    fun maxUniqueSplit(s: String): Int {
        var lo = 1
        var hi = s.length
        // binary search
        while (lo < hi) {
            val mid = lo + hi + 1 shr 1
            if (ok(0, mid, 0, s, HashSet())) {
                lo = mid
            } else {
                hi = mid - 1
            }
        }
        return lo
    }

    private fun ok(depth: Int, end: Int, curLen: Int, s: String, seen: MutableSet<String>): Boolean {
        if (depth == end) {
            return true
        }
        for (j in curLen until s.length) {
            // not enough length remains to reach the end.
            if (s.length - j < end - depth) {
                break
            }
            val cur = s.substring(curLen, j + 1)
            if (seen.add(cur)) {
                if (ok(depth + 1, end, j + 1, s, seen)) {
                    return true
                }
                seen.remove(cur)
            }
        }
        return false
    }
}
```