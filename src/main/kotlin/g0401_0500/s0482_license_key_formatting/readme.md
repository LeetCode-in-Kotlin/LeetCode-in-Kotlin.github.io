[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 482\. License Key Formatting

Easy

You are given a license key represented as a string `s` that consists of only alphanumeric characters and dashes. The string is separated into `n + 1` groups by `n` dashes. You are also given an integer `k`.

We want to reformat the string `s` such that each group contains exactly `k` characters, except for the first group, which could be shorter than `k` but still must contain at least one character. Furthermore, there must be a dash inserted between two groups, and you should convert all lowercase letters to uppercase.

Return _the reformatted license key_.

**Example 1:**

**Input:** s = "5F3Z-2e-9-w", k = 4

**Output:** "5F3Z-2E9W"

**Explanation:** The string s has been split into two parts, each part has 4 characters. Note that the two extra dashes are not needed and can be removed.

**Example 2:**

**Input:** s = "2-5g-3-J", k = 2

**Output:** "2-5G-3J"

**Explanation:** The string s has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of English letters, digits, and dashes `'-'`.
*   <code>1 <= k <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun licenseKeyFormatting(s: String, k: Int): String {
        val sb = StringBuilder()
        var cnt = 0
        var occ = 0
        for (c in s.toCharArray()) {
            if (c == '-') {
                continue
            }
            cnt++
        }
        var l = cnt % k
        for (c in s.toCharArray()) {
            if (c == '-') {
                continue
            }
            if (occ == k) {
                occ = 0
                sb.append('-')
            } else if (occ == l && l != 0) {
                l = 0
                occ = 0
                sb.append('-')
            }
            sb.append(c.uppercaseChar())
            occ++
        }
        return sb.toString()
    }
}
```