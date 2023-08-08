[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2719\. Count of Integers

Hard

You are given two numeric strings `num1` and `num2` and two integers `max_sum` and `min_sum`. We denote an integer `x` to be _good_ if:

*   `num1 <= x <= num2`
*   `min_sum <= digit_sum(x) <= max_sum`.

Return _the number of good integers_. Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

Note that `digit_sum(x)` denotes the sum of the digits of `x`.

**Example 1:**

**Input:** num1 = "1", num2 = "12", `min_sum` = 1, max\_sum = 8

**Output:** 11

**Explanation:** There are 11 integers whose sum of digits lies between 1 and 8 are 1,2,3,4,5,6,7,8,10,11, and 12. Thus, we return 11.

**Example 2:**

**Input:** num1 = "1", num2 = "5", `min_sum` = 1, max\_sum = 5

**Output:** 5

**Explanation:** The 5 integers whose sum of digits lies between 1 and 5 are 1,2,3,4, and 5. Thus, we return 5.

**Constraints:**

*   <code>1 <= num1 <= num2 <= 10<sup>22</sup></code>
*   `1 <= min_sum <= max_sum <= 400`

## Solution

```kotlin
import java.util.Arrays

class Solution {
    private lateinit var dp: Array<Array<Array<IntArray>>>
    private fun countStrings(i: Int, tight1: Boolean, tight2: Boolean, sum: Int, num1: String, num2: String): Int {
        if (sum < 0) return 0
        if (i == num2.length) return 1
        if (dp[i][if (tight1) 1 else 0][if (tight2) 1 else 0][sum] != -1)
            return dp[i][if (tight1) 1 else 0][if (tight2) 1 else 0][sum]
        val lo = if (tight1) num1[i].code - '0'.code else 0
        val hi = if (tight2) num2[i].code - '0'.code else 9
        var count = 0
        for (idx in lo..hi) {
            count = (
                count % MOD + countStrings(
                    i + 1,
                    tight1 and (idx == lo), tight2 and (idx == hi),
                    sum - idx, num1, num2
                ) % MOD
                ) % MOD
        }
        return count.also { dp[i][if (tight1) 1 else 0][if (tight2) 1 else 0][sum] = it }
    }

    fun count(num1: String, num2: String, minSum: Int, maxSum: Int): Int {
        val maxLength = num2.length
        val minLength = num1.length
        val leadingZeroes = maxLength - minLength
        val num1extended: String = "0".repeat(leadingZeroes) + num1
        dp = Array(maxLength) {
            Array(2) {
                Array(2) {
                    IntArray(401)
                }
            }
        }
        for (dim1 in dp) {
            for (dim2 in dim1) {
                for (dim3 in dim2) {
                    Arrays.fill(dim3, -1)
                }
            }
        }
        val total = countStrings(0, true, true, maxSum, num1extended, num2)
        val unnecessary = countStrings(0, true, true, minSum - 1, num1extended, num2)
        var ans = (total - unnecessary) % MOD
        if (ans < 0) {
            ans += MOD
        }
        return ans
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```