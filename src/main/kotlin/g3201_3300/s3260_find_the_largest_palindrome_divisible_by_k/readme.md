[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3260\. Find the Largest Palindrome Divisible by K

Hard

You are given two **positive** integers `n` and `k`.

An integer `x` is called **k-palindromic** if:

*   `x` is a palindrome.
*   `x` is divisible by `k`.

Return the **largest** integer having `n` digits (as a string) that is **k-palindromic**.

**Note** that the integer must **not** have leading zeros.

**Example 1:**

**Input:** n = 3, k = 5

**Output:** "595"

**Explanation:**

595 is the largest k-palindromic integer with 3 digits.

**Example 2:**

**Input:** n = 1, k = 4

**Output:** "8"

**Explanation:**

4 and 8 are the only k-palindromic integers with 1 digit.

**Example 3:**

**Input:** n = 5, k = 6

**Output:** "89898"

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= k <= 9`

## Solution

```kotlin
class Solution {
    fun largestPalindrome(n: Int, k: Int): String {
        val sc = CharArray(n)
        if (k == 1 || k == 3 || k == 9) {
            sc.fill('9')
        } else if (k == 7) {
            if (n == 1) {
                return "7"
            } else if (n == 2) {
                return "77"
            }
            val mod = n % 12
            checkValues(n, mod, sc)
        } else if (k == 2) {
            sc.fill('9')
            sc[0] = '8'
            sc[n - 1] = '8'
        } else if (k == 4) {
            sc.fill('8')
            var i = 2
            var j = n - 3
            while (i <= j) {
                sc[i] = '9'
                sc[j] = '9'
                ++i
                --j
            }
        } else if (k == 5) {
            sc.fill('9')
            sc[0] = '5'
            sc[n - 1] = '5'
        } else if (k == 6) {
            val number = getString(n, sc)
            if (number != null) {
                return number
            }
        } else if (k == 8) {
            sc.fill('8')
            var i = 3
            var j = n - 4
            while (i <= j) {
                sc[i] = '9'
                sc[j] = '9'
                ++i
                --j
            }
        }
        return String(sc)
    }

    private fun checkValues(n: Int, mod: Int, sc: CharArray) {
        if (mod == 6 || mod == 0) {
            sc.fill('9')
        } else if (mod == 3) {
            sc.fill('9')
            sc[n / 2] = '5'
        } else if (mod == 4 || mod == 5 || mod == 1 || mod == 2) {
            sc.fill('7')
            var i = 0
            var j = n - 1
            while (i + 1 < j) {
                sc[i] = '9'
                sc[j] = '9'
                ++i
                --j
            }
        } else if (mod == 7 || mod == 8 || mod == 10 || mod == 11) {
            sc.fill('4')
            var i = 0
            var j = n - 1
            while (i + 1 < j) {
                sc[i] = '9'
                sc[j] = '9'
                ++i
                --j
            }
        } else if (mod == 9) {
            sc.fill('9')
            sc[n / 2] = '6'
        }
    }

    private fun getString(n: Int, sc: CharArray): String? {
        if (n == 1) {
            return "6"
        } else if (n == 2) {
            return "66"
        } else {
            if (n % 2 == 1) {
                sc.fill('9')
                sc[0] = '8'
                sc[n - 1] = '8'
                sc[n / 2] = '8'
            } else {
                sc.fill('9')
                sc[0] = '8'
                sc[n - 1] = '8'
                sc[n / 2] = '7'
                sc[n / 2 - 1] = '7'
            }
        }
        return null
    }
}
```