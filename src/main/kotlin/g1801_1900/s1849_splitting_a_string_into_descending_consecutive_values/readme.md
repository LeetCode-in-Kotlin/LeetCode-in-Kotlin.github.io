[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1849\. Splitting a String Into Descending Consecutive Values

Medium

You are given a string `s` that consists of only digits.

Check if we can split `s` into **two or more non-empty substrings** such that the **numerical values** of the substrings are in **descending order** and the **difference** between numerical values of every two **adjacent** **substrings** is equal to `1`.

*   For example, the string `s = "0090089"` can be split into `["0090", "089"]` with numerical values `[90,89]`. The values are in descending order and adjacent values differ by `1`, so this way is valid.
*   Another example, the string `s = "001"` can be split into `["0", "01"]`, `["00", "1"]`, or `["0", "0", "1"]`. However all the ways are invalid because they have numerical values `[0,1]`, `[0,1]`, and `[0,0,1]` respectively, all of which are not in descending order.

Return `true` _if it is possible to split_ `s` _as described above__, or_ `false` _otherwise._

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** s = "1234"

**Output:** false

**Explanation:** There is no valid way to split s.

**Example 2:**

**Input:** s = "050043"

**Output:** true

**Explanation:** s can be split into ["05", "004", "3"] with numerical values [5,4,3]. The values are in descending order with adjacent values differing by 1.

**Example 3:**

**Input:** s = "9080701"

**Output:** false

**Explanation:** There is no valid way to split s.

**Constraints:**

*   `1 <= s.length <= 20`
*   `s` only consists of digits.

## Solution

```kotlin
class Solution {
    fun splitString(s: String): Boolean {
        return solve(0, -1, s, 0)
    }

    private fun solve(i: Int, prev: Long, s: String, k: Int): Boolean {
        if (i == s.length) {
            return k >= 2
        }
        var cur: Long = 0
        for (j in i until s.length) {
            cur = cur * 10 + s[j].code.toLong() - '0'.code.toLong()
            if ((prev == -1L || prev - cur == 1L) && solve(j + 1, cur, s, k + 1)) {
                return true
            }
        }
        return false
    }
}
```