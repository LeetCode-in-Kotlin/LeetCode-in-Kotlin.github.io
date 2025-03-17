[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3490\. Count Beautiful Numbers

Hard

You are given two positive integers, `l` and `r`. A positive integer is called **beautiful** if the product of its digits is divisible by the sum of its digits.

Return the count of **beautiful** numbers between `l` and `r`, inclusive.

**Example 1:**

**Input:** l = 10, r = 20

**Output:** 2

**Explanation:**

The beautiful numbers in the range are 10 and 20.

**Example 2:**

**Input:** l = 1, r = 15

**Output:** 10

**Explanation:**

The beautiful numbers in the range are 1, 2, 3, 4, 5, 6, 7, 8, 9, and 10.

**Constraints:**

*   <code>1 <= l <= r < 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun beautifulNumbers(l: Int, r: Int): Int {
        return countBeautiful(r) - countBeautiful(l - 1)
    }

    private fun countBeautiful(x: Int): Int {
        val digits = getCharArray(x)
        val dp = HashMap<String?, Int?>()
        return solve(0, 1, 0, 1, digits, dp)
    }

    private fun getCharArray(x: Int): CharArray {
        val str = x.toString()
        return str.toCharArray()
    }

    private fun solve(
        i: Int,
        tight: Int,
        sum: Int,
        prod: Int,
        digits: CharArray,
        dp: HashMap<String?, Int?>,
    ): Int {
        if (i == digits.size) {
            return if (sum > 0 && prod % sum == 0) {
                1
            } else {
                0
            }
        }
        val str = "$i - $tight - $sum - $prod"
        if (dp.containsKey(str)) {
            return dp[str]!!
        }
        val limit: Int = if (tight == 1) {
            digits[i].code - '0'.code
        } else {
            9
        }
        var count = 0
        var j = 0
        while (j <= limit) {
            var newTight = 0
            if (tight == 1 && j == limit) {
                newTight = 1
            }
            val newSum = sum + j
            val newProd: Int = if (j == 0 && sum == 0) {
                1
            } else {
                prod * j
            }
            count += solve(i + 1, newTight, newSum, newProd, digits, dp)
            j++
        }
        dp.put(str, count)
        return count
    }
}
```