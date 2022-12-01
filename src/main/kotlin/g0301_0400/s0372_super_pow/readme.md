[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 372\. Super Pow

Medium

Your task is to calculate <code>a<sup>b</sup></code> mod `1337` where `a` is a positive integer and `b` is an extremely large positive integer given in the form of an array.

**Example 1:**

**Input:** a = 2, b = [3]

**Output:** 8

**Example 2:**

**Input:** a = 2, b = [1,0]

**Output:** 1024

**Example 3:**

**Input:** a = 1, b = [4,3,3,8,5,2]

**Output:** 1

**Constraints:**

*   <code>1 <= a <= 2<sup>31</sup> - 1</code>
*   `1 <= b.length <= 2000`
*   `0 <= b[i] <= 9`
*   `b` does not contain leading zeros.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun superPow(a: Int, b: IntArray): Int {
        val phi = phi(MOD)
        val arrMod = arrMod(b, phi)
        return if (isGreaterOrEqual(b, phi)) {
            // Cycle has started
            // cycle starts at phi with length phi
            exp(a % MOD, phi + arrMod)
        } else exp(a % MOD, arrMod)
    }

    private fun phi(n: Int): Int {
        var n = n
        var result = n.toDouble()
        var p = 2
        while (p * p <= n) {
            if (n % p > 0) {
                p++
                continue
            }
            while (n % p == 0) {
                n /= p
            }
            result *= 1.0 - 1.0 / p
            p++
        }
        if (n > 1) {
            // if starting n was also prime (so it was greater than sqrt(n))
            result *= 1.0 - 1.0 / n
        }
        return result.toInt()
    }

    // Returns true if number in array is greater than integer named phi
    private fun isGreaterOrEqual(b: IntArray, phi: Int): Boolean {
        var cur = 0
        for (j in b) {
            cur = cur * 10 + j
            if (cur >= phi) {
                return true
            }
        }
        return false
    }

    // Returns number in array mod phi
    private fun arrMod(b: IntArray, phi: Int): Int {
        var res = 0
        for (j in b) {
            res = (res * 10 + j) % phi
        }
        return res
    }

    // Binary exponentiation
    private fun exp(a: Int, b: Int): Int {
        var a = a
        var b = b
        var y = 1
        while (b > 0) {
            if (b % 2 == 1) {
                y = y * a % MOD
            }
            a = a * a % MOD
            b /= 2
        }
        return y
    }

    companion object {
        private const val MOD = 1337
    }
}
```