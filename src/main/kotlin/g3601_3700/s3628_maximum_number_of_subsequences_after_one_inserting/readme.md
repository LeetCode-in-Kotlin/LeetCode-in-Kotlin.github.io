[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3628\. Maximum Number of Subsequences After One Inserting

Medium

You are given a string `s` consisting of uppercase English letters.

You are allowed to insert **at most one** uppercase English letter at **any** position (including the beginning or end) of the string.

Return the **maximum** number of `"LCT"` subsequences that can be formed in the resulting string after **at most one insertion**.

**Example 1:**

**Input:** s = "LMCT"

**Output:** 2

**Explanation:**

We can insert a `"L"` at the beginning of the string s to make `"LLMCT"`, which has 2 subsequences, at indices [0, 3, 4] and [1, 3, 4].

**Example 2:**

**Input:** s = "LCCT"

**Output:** 4

**Explanation:**

We can insert a `"L"` at the beginning of the string s to make `"LLCCT"`, which has 4 subsequences, at indices [0, 2, 4], [0, 3, 4], [1, 2, 4] and [1, 3, 4].

**Example 3:**

**Input:** s = "L"

**Output:** 0

**Explanation:**

Since it is not possible to obtain the subsequence `"LCT"` by inserting a single letter, the result is 0.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of uppercase English letters.

## Solution

```kotlin
class Solution {
    fun numOfSubsequences(s: String): Long {
        var tc: Long = 0
        val chs = s.toCharArray()
        for (c in chs) {
            tc += (if (c == 'T') 1 else 0).toLong()
        }
        var ls: Long = 0
        var cs: Long = 0
        var lcf: Long = 0
        var ctf: Long = 0
        var lct: Long = 0
        var ocg: Long = 0
        var tp: Long = 0
        for (curr in chs) {
            val rt = tc - tp
            val cg = ls * rt
            ocg = if (cg > ocg) cg else ocg
            if (curr == 'L') {
                ls++
            } else {
                if (curr == 'C') {
                    cs++
                    lcf += ls
                } else {
                    if (curr == 'T') {
                        lct += lcf
                        ctf += cs
                        tp++
                    }
                }
            }
        }
        val fcg = ls * (tc - tp)
        ocg = if (fcg > ocg) fcg else ocg
        var maxi: Long = 0
        val bo = longArrayOf(lcf, ctf, ocg)
        for (op in bo) {
            maxi = if (op > maxi) op else maxi
        }
        return lct + maxi
    }
}
```