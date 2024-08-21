[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3258\. Count Substrings That Satisfy K-Constraint I

Easy

You are given a **binary** string `s` and an integer `k`.

A **binary string** satisfies the **k-constraint** if **either** of the following conditions holds:

*   The number of `0`'s in the string is at most `k`.
*   The number of `1`'s in the string is at most `k`.

Return an integer denoting the number of substrings of `s` that satisfy the **k-constraint**.

**Example 1:**

**Input:** s = "10101", k = 1

**Output:** 12

**Explanation:**

Every substring of `s` except the substrings `"1010"`, `"10101"`, and `"0101"` satisfies the k-constraint.

**Example 2:**

**Input:** s = "1010101", k = 2

**Output:** 25

**Explanation:**

Every substring of `s` except the substrings with a length greater than 5 satisfies the k-constraint.

**Example 3:**

**Input:** s = "11111", k = 1

**Output:** 15

**Explanation:**

All substrings of `s` satisfy the k-constraint.

**Constraints:**

*   `1 <= s.length <= 50`
*   `1 <= k <= s.length`
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun countKConstraintSubstrings(s: String, k: Int): Int {
        val n = s.length
        var sum = n
        var i = 0
        var j = 0
        var one = 0
        var zero = 0
        var ch: Char
        while (j < n) {
            ch = s[j++]
            if (ch == '0') {
                zero++
            } else {
                one++
            }
            while (i <= j && one > k && zero > k) {
                ch = s[i++]
                if (ch == '0') {
                    zero--
                } else {
                    one--
                }
            }
            val len = (zero + one - 1)
            sum += len
        }
        return sum
    }
}
```