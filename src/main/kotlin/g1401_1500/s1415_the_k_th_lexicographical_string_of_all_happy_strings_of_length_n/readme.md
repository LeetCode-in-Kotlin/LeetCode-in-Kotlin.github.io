[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1415\. The k-th Lexicographical String of All Happy Strings of Length n

Medium

A **happy string** is a string that:

*   consists only of letters of the set `['a', 'b', 'c']`.
*   `s[i] != s[i + 1]` for all values of `i` from `1` to `s.length - 1` (string is 1-indexed).

For example, strings **"abc", "ac", "b"** and **"abcbabcbcb"** are all happy strings and strings **"aa", "baa"** and **"ababbc"** are not happy strings.

Given two integers `n` and `k`, consider a list of all happy strings of length `n` sorted in lexicographical order.

Return _the kth string_ of this list or return an **empty string** if there are less than `k` happy strings of length `n`.

**Example 1:**

**Input:** n = 1, k = 3

**Output:** "c"

**Explanation:** The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".

**Example 2:**

**Input:** n = 1, k = 4

**Output:** ""

**Explanation:** There are only 3 happy strings of length 1.

**Example 3:**

**Input:** n = 3, k = 9

**Output:** "cab"

**Explanation:** There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9<sup>th</sup> string = "cab"

**Constraints:**

*   `1 <= n <= 10`
*   `1 <= k <= 100`

## Solution

```kotlin
class Solution {
    private val arr = charArrayOf('a', 'b', 'c')
    private var res = ""
    private var k = 0

    private operator fun get(str: StringBuilder, n: Int, index: Int) {
        if (k < 1) {
            return
        }
        if (str.length == n) {
            if (k == 1) {
                res = str.toString()
            }
            k--
            return
        }
        for (i in 0..2) {
            if (i == index) {
                continue
            }
            str.append(arr[i])
            get(str, n, i)
            str.deleteCharAt(str.length - 1)
        }
    }

    fun getHappyString(n: Int, k: Int): String {
        this.k = k
        get(StringBuilder(), n, -1)
        return res
    }
}
```