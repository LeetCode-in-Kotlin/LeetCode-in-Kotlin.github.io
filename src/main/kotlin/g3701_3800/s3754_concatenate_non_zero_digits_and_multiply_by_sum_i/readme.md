[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3754\. Concatenate Non-Zero Digits and Multiply by Sum I

Easy

You are given an integer `n`.

Form a new integer `x` by concatenating all the **non-zero digits** of `n` in their original order. If there are no **non-zero** digits, `x = 0`.

Let `sum` be the **sum of digits** in `x`.

Return an integer representing the value of `x * sum`.

**Example 1:**

**Input:** n = 10203004

**Output:** 12340

**Explanation:**

*   The non-zero digits are 1, 2, 3, and 4. Thus, `x = 1234`.
*   The sum of digits is `sum = 1 + 2 + 3 + 4 = 10`.
*   Therefore, the answer is `x * sum = 1234 * 10 = 12340`.

**Example 2:**

**Input:** n = 1000

**Output:** 1

**Explanation:**

*   The non-zero digit is 1, so `x = 1` and `sum = 1`.
*   Therefore, the answer is `x * sum = 1 * 1 = 1`.

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun sumAndMultiply(n: Int): Long {
        var newNum = 0
        var y = abs(n)
        while (y > 0) {
            val rem = y % 10
            if (rem != 0) {
                newNum = newNum * 10 + rem
            }
            y /= 10
        }
        var temp = 0
        while (newNum > 0) {
            val rem = newNum % 10
            temp = temp * 10 + rem
            newNum /= 10
        }
        val x = temp
        var sum: Long = 0
        while (temp > 0) {
            val rem = temp % 10
            sum += rem.toLong()
            temp /= 10
        }
        return sum * x
    }
}
```