[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3079\. Find the Sum of Encrypted Integers

Easy

You are given an integer array `nums` containing **positive** integers. We define a function `encrypt` such that `encrypt(x)` replaces **every** digit in `x` with the **largest** digit in `x`. For example, `encrypt(523) = 555` and `encrypt(213) = 333`.

Return _the **sum** of encrypted elements_.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 6

**Explanation:** The encrypted elements are `[1,2,3]`. The sum of encrypted elements is `1 + 2 + 3 == 6`.

**Example 2:**

**Input:** nums = [10,21,31]

**Output:** 66

**Explanation:** The encrypted elements are `[11,22,33]`. The sum of encrypted elements is `11 + 22 + 33 == 66`.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `1 <= nums[i] <= 1000`

## Solution

```kotlin
import kotlin.math.max

@Suppress("NAME_SHADOWING")
class Solution {
    private fun encrypt(x: Int): Int {
        var x = x
        var nDigits = 0
        var max = 0
        while (x > 0) {
            max = max(max, (x % 10))
            x /= 10
            nDigits++
        }
        var ans = 0
        for (i in 0 until nDigits) {
            ans = ans * 10 + max
        }
        return ans
    }

    fun sumOfEncryptedInt(nums: IntArray): Int {
        var ret = 0
        for (num in nums) {
            ret += encrypt(num)
        }
        return ret
    }
}
```