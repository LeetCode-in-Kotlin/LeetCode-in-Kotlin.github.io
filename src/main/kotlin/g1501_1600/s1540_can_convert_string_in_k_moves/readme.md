[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1540\. Can Convert String in K Moves

Medium

Given two strings `s` and `t`, your goal is to convert `s` into `t` in `k`moves or less.

During the <code>i<sup>th</sup></code> (`1 <= i <= k`) move you can:

*   Choose any index `j` (1-indexed) from `s`, such that `1 <= j <= s.length` and `j` has not been chosen in any previous move, and shift the character at that index `i` times.
*   Do nothing.

Shifting a character means replacing it by the next letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Shifting a character by `i` means applying the shift operations `i` times.

Remember that any index `j` can be picked at most once.

Return `true` if it's possible to convert `s` into `t` in no more than `k` moves, otherwise return `false`.

**Example 1:**

**Input:** s = "input", t = "ouput", k = 9

**Output:** true

**Explanation:** In the 6th move, we shift 'i' 6 times to get 'o'. And in the 7th move we shift 'n' to get 'u'.

**Example 2:**

**Input:** s = "abc", t = "bcd", k = 10

**Output:** false

**Explanation:** We need to shift each character in s one time to convert it into t. We can shift 'a' to 'b' during the 1st move. However, there is no way to shift the other characters in the remaining moves to obtain t from s.

**Example 3:**

**Input:** s = "aab", t = "bbb", k = 27

**Output:** true

**Explanation:** In the 1st move, we shift the first 'a' 1 time to get 'b'. In the 27th move, we shift the second 'a' 27 times to get 'b'.

**Constraints:**

*   `1 <= s.length, t.length <= 10^5`
*   `0 <= k <= 10^9`
*   `s`, `t` contain only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun canConvertString(s: String, t: String, k: Int): Boolean {
        val len1 = s.length
        val len2 = t.length
        if (len1 != len2) {
            return false
        }
        if (s == t) {
            return true
        }
        val freq = IntArray(26)
        val multiple = k / 26
        for (i in 0..25) {
            freq[i] = multiple
        }
        val rem = k % 26
        for (i in 1..rem) {
            freq[i]++
        }
        var movesRemaining = k
        for (i in 0 until len1) {
            val ch1 = s[i]
            val ch2 = t[i]
            if (ch1 == ch2) {
                movesRemaining--
                continue
            }
            val diff = (ch2.code - ch1.code + 26) % 26
            if (freq[diff] > 0) {
                freq[diff]--
                movesRemaining--
            } else {
                return false
            }
        }
        return movesRemaining >= 0
    }
}
```