[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1573\. Number of Ways to Split a String

Medium

Given a binary string `s`, you can split `s` into 3 **non-empty** strings `s1`, `s2`, and `s3` where `s1 + s2 + s3 = s`.

Return the number of ways `s` can be split such that the number of ones is the same in `s1`, `s2`, and `s3`. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "10101"

**Output:** 4

**Explanation:** There are four ways to split s in 3 parts where each part contain the same number of letters '1'. 

"1\|010\|1" 

"1\|01\|01" 

"10\|10\|1" 

"10\|1\|01"

**Example 2:**

**Input:** s = "1001"

**Output:** 0

**Example 3:**

**Input:** s = "0000"

**Output:** 3

**Explanation:** There are three ways to split s in 3 parts. 

"0\|0\|00" 

"0\|00\|0" 

"00\|0\|0"

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun numWays(s: String): Int {
        var totalOnesCount: Long = 0
        val mod: Long = 1000000007
        var waysOfFirstString: Long = 0
        var waysOfSecondString: Long = 0
        var onesCount: Long = 0
        val n = s.length.toLong()
        for (i in 0 until s.length) {
            if (s[i] == '1') {
                totalOnesCount += 1
            }
        }
        if (totalOnesCount % 3 != 0L) {
            return 0
        }
        val onesFirstPart = totalOnesCount / 3
        val onesSecondPart = onesFirstPart * 2
        if (totalOnesCount == 0L) {
            return ((n - 1) * (n - 2) / 2 % mod).toInt()
        }
        for (i in 0 until s.length) {
            if (s[i] == '1') {
                onesCount += 1
            }
            if (onesCount == onesFirstPart) {
                waysOfFirstString += 1
            } else if (onesCount == onesSecondPart) {
                waysOfSecondString += 1
            } else if (onesCount > onesSecondPart) {
                break
            }
        }
        return (waysOfFirstString * waysOfSecondString % mod).toInt()
    }
}
```