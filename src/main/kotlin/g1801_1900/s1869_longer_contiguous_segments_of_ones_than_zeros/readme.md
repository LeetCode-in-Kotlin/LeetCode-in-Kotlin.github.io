[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1869\. Longer Contiguous Segments of Ones than Zeros

Easy

Given a binary string `s`, return `true` _if the **longest** contiguous segment of_ `1`'_s is **strictly longer** than the **longest** contiguous segment of_ `0`'_s in_ `s`, or return `false` _otherwise_.

*   For example, in `s = "110100010"` the longest continuous segment of `1`s has length `2`, and the longest continuous segment of `0`s has length `3`.

Note that if there are no `0`'s, then the longest continuous segment of `0`'s is considered to have a length `0`. The same applies if there is no `1`'s.

**Example 1:**

**Input:** s = "1101"

**Output:** true

**Explanation:** 

The longest contiguous segment of 1s has length 2: "1101" 

The longest contiguous segment of 0s has length 1: "1101" 

The segment of 1s is longer, so return true.

**Example 2:**

**Input:** s = "111000"

**Output:** false

**Explanation:** 

The longest contiguous segment of 1s has length 3: "111000" 

The longest contiguous segment of 0s has length 3: "111000" 

The segment of 1s is not longer, so return false.

**Example 3:**

**Input:** s = "110100010"

**Output:** false

**Explanation:**

The longest contiguous segment of 1s has length 2: "110100010" 

The longest contiguous segment of 0s has length 3: "110100010" 

The segment of 1s is not longer, so return false.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun checkZeroOnes(s: String): Boolean {
        var zeroes = 0
        var ones = 0
        var i = 0
        while (i < s.length) {
            var start = i
            while (i < s.length && s[i] == '0') {
                i++
            }
            if (i > start) {
                zeroes = Math.max(zeroes, i - start)
            }
            start = i
            while (i < s.length && s[i] == '1') {
                i++
            }
            if (i > start) {
                ones = Math.max(ones, i - start)
            }
        }
        return ones > zeroes
    }
}
```