[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3922\. Minimum Flips to Make Binary String Coherent

Medium

You are given a binary string `s`.

A string is considered **coherent** if it does **not** contain `"011"` or `"110"` as subsequences.

In one operation, you can **flip** any character in `s` (`'0'` to `'1'` or `'1'` to `'0'`).

Return an integer denoting the **minimum** number of operations required to make `s` coherent.

**Example 1:**

**Input:** s = "1010"

**Output:** 1

**Explanation:**

Flip `s[0]` to get `"0010"`, which contains no `"011"` or `"110"` subsequences.

**Example 2:**

**Input:** s = "0110"

**Output:** 1

**Explanation:**

Flip `s[1]` to get `"0010"`, removing all forbidden subsequences `"011"` and `"110"`.

**Example 3:**

**Input:** s = "1000"

**Output:** 0

**Explanation:**

The string already has no `"011"` or `"110"` subsequences, so no flips are needed.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```kotlin
import kotlin.math.min


class Solution {
    fun minFlips(s: String): Int {
        val n = s.length
        var count1 = 0
        for (c in s.toCharArray()) {
            count1 += c.code - '0'.code
        }
        val count0 = n - count1
        // Option 1: all 0s
        var ans = count1
        // Option 2: all 1s
        ans = min(ans, count0)
        // Option 3: exactly one 1 (at any position)
        ans = if (count1 >= 1) {
            min(ans, count1 - 1)
        } else {
            min(ans, 1)
        }
        // Option 4: "10...01" (1 at position 0 and position n-1)
        if (n >= 2 && count1 >= 2) {
            var cost4 = count1
            if (s[0] == '1') {
                cost4--
            }
            if (s[n - 1] == '1') {
                cost4--
            }
            ans = min(ans, cost4)
        }
        return ans
    }
}
```