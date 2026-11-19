[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4021\. Minimum Operations to Make a Rotated Palindrome I

Medium

You are given a string `s` consisting of lowercase English letters.

You can perform the following operations any number of times (including zero) and in any order:

*   **Increment**: Choose any index `i` and replace `s[i]` with the next lowercase English letter. The letter after `'z'` is `'a'`.
*   **Left rotate**: Move the first character of the string to the end.

Return the **minimum** number of operations required to make `s` a palindrome.

**Example 1:**

**Input:** s = "abc"

**Output:** 2

**Explanation:**

One optimal solution:

*   Left rotate the string: `"abc" -> "bca"`.
*   Increment `'a'` to `'b'`: `"bca" -> "bcb"`.
*   `"bcb"` is a palindrome. Thus, the answer is 2.

**Example 2:**

**Input:** s = "yb"

**Output:** 3

**Explanation:**

*   Increment the first character three times: `"yb" -> "zb" -> "ab" -> "bb"`.
*   `"bb"` is a palindrome. Thus, the answer is 3.

**Constraints:**

*   `2 <= s.length <= 2000`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.min

class Solution {
    fun minOperations(s: String): Int {
        val chars = s.toCharArray()
        val n = s.length
        val dist = Array<IntArray?>(26) { IntArray(26) }
        for (a in 0..25) {
            for (b in 0..25) {
                val diff = abs(a - b)
                dist[a]!![b] = min(diff, 26 - diff)
            }
        }
        val vals = IntArray(n)
        for (i in 0..<n) {
            vals[i] = chars[i].code - 97
        }
        val mid = n shr 1
        var min = 0
        for (i in 0..<mid) {
            min += dist[vals[i]]!![vals[n - 1 - i]]
        }
        val diff = IntArray(n shl 1)
        for (i in 0..<2 * n) {
            diff[i] = vals[i % n]
        }
        var shift = 1
        while (shift < n shr (1 - (n and 1)) && shift < min) {
            var curr = shift
            for (i in 0..<mid) {
                curr += dist[diff[shift + i]]!![diff[shift + n - 1 - i]]
                if (curr >= min) {
                    break
                }
            }
            min = min(min, curr)
            ++shift
        }
        return min
    }
}
```