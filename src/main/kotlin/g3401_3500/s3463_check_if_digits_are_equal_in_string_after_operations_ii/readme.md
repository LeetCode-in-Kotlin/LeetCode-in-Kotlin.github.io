[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3463\. Check If Digits Are Equal in String After Operations II

Hard

You are given a string `s` consisting of digits. Perform the following operation repeatedly until the string has **exactly** two digits:

*   For each pair of consecutive digits in `s`, starting from the first digit, calculate a new digit as the sum of the two digits **modulo** 10.
*   Replace `s` with the sequence of newly calculated digits, _maintaining the order_ in which they are computed.

Return `true` if the final two digits in `s` are the **same**; otherwise, return `false`.

**Example 1:**

**Input:** s = "3902"

**Output:** true

**Explanation:**

*   Initially, `s = "3902"`
*   First operation:
    *   `(s[0] + s[1]) % 10 = (3 + 9) % 10 = 2`
    *   `(s[1] + s[2]) % 10 = (9 + 0) % 10 = 9`
    *   `(s[2] + s[3]) % 10 = (0 + 2) % 10 = 2`
    *   `s` becomes `"292"`
*   Second operation:
    *   `(s[0] + s[1]) % 10 = (2 + 9) % 10 = 1`
    *   `(s[1] + s[2]) % 10 = (9 + 2) % 10 = 1`
    *   `s` becomes `"11"`
*   Since the digits in `"11"` are the same, the output is `true`.

**Example 2:**

**Input:** s = "34789"

**Output:** false

**Explanation:**

*   Initially, `s = "34789"`.
*   After the first operation, `s = "7157"`.
*   After the second operation, `s = "862"`.
*   After the third operation, `s = "48"`.
*   Since `'4' != '8'`, the output is `false`.

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only digits.

## Solution

```kotlin
class Solution {
    private fun powMod10(a: Int, n: Int): Int {
        var a = a
        var n = n
        var x = 1
        while (n >= 1) {
            if (n % 2 == 1) {
                x = (x * a) % 10
            }
            a = (a * a) % 10
            n /= 2
        }
        return x
    }

    private fun f(n: Int): IntArray {
        val ns = IntArray(n + 1)
        val n2 = IntArray(n + 1)
        val n5 = IntArray(n + 1)
        ns[0] = 1
        for (i in 1..n) {
            var m = i
            n2[i] = n2[i - 1]
            n5[i] = n5[i - 1]
            while (m % 2 == 0) {
                m /= 2
                n2[i]++
            }
            while (m % 5 == 0) {
                m /= 5
                n5[i]++
            }
            ns[i] = (ns[i - 1] * m) % 10
        }
        val inv = IntArray(10)
        for (i in 1..9) {
            for (j in 0..9) {
                if (i * j % 10 == 1) {
                    inv[i] = j
                }
            }
        }
        val xs = IntArray(n + 1)
        for (k in 0..n) {
            var a = 0
            val s2 = n2[n] - n2[n - k] - n2[k]
            val s5 = n5[n] - n5[n - k] - n5[k]
            if (s2 == 0 || s5 == 0) {
                a = (ns[n] * inv[ns[n - k]] * inv[ns[k]] * powMod10(2, s2) * powMod10(5, s5)) % 10
            }
            xs[k] = a
        }
        return xs
    }

    fun hasSameDigits(s: String): Boolean {
        val n = s.length
        val xs = f(n - 2)
        val arr = IntArray(n)
        for (i in 0..<n) {
            arr[i] = s[i].code - '0'.code
        }
        var num1 = 0
        var num2 = 0
        for (i in 0..<n - 1) {
            num1 = (num1 + xs[i] * arr[i]) % 10
            num2 = (num2 + xs[i] * arr[i + 1]) % 10
        }
        return num1 == num2
    }
}
```