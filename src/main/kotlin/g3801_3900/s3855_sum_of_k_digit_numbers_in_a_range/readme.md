[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3855\. Sum of K-Digit Numbers in a Range

Hard

You are given three integers `l`, `r`, and `k`.

Consider all possible integers consisting of **exactly** `k` digits, where each digit is chosen independently from the integer range `[l, r]` (inclusive). If 0 is included in the range, leading zeros are allowed.

Return an integer representing the **sum of all such numbers.** Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** l = 1, r = 2, k = 2

**Output:** 66

**Explanation:**

*   All numbers formed using `k = 2` digits in the range `[1, 2]` are `11, 12, 21, 22`.
*   The total sum is `11 + 12 + 21 + 22 = 66`.

**Example 2:**

**Input:** l = 0, r = 1, k = 3

**Output:** 444

**Explanation:**

*   All numbers formed using `k = 3` digits in the range `[0, 1]` are `000, 001, 010, 011, 100, 101, 110, 111`.
*   These numbers without leading zeros are `0, 1, 10, 11, 100, 101, 110, 111`.
*   The total sum is 444.

**Example 3:**

**Input:** l = 5, r = 5, k = 10

**Output:** 555555520

**Explanation:**

*   5555555555 is the only valid number consisting of `k = 10` digits in the range `[5, 5]`.
*   The total sum is <code>5555555555 % (10<sup>9</sup> + 7) = 555555520</code>.

**Constraints:**

*   `0 <= l <= r <= 9`
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun sumOfNumbers(l: Int, r: Int, k: Int): Int {
        val count = r - l.toLong() + 1
        val sumRange = (l + r) * count / 2
        val t1: Long = sumRange % MOD
        val t2 = power(count, k.toLong() - 1)
        val repunit: Long = (power(10, k.toLong()) - 1 + MOD) % MOD
        val inv9 = power(9, MOD.toLong() - 2)
        val t3: Long = (repunit * inv9) % MOD
        var ans: Long = (t1 * t2) % MOD
        ans = (ans * t3) % MOD
        return ans.toInt()
    }

    private fun power(base: Long, exp: Long): Long {
        var base = base
        var exp = exp
        var res: Long = 1
        base %= MOD.toLong()
        while (exp > 0) {
            if (exp % 2 == 1L) {
                res = (res * base) % MOD
            }
            base = (base * base) % MOD
            exp /= 2
        }
        return res
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```