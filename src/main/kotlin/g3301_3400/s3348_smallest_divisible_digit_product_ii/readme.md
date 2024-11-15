[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3348\. Smallest Divisible Digit Product II

Hard

You are given a string `num` which represents a **positive** integer, and an integer `t`.

A number is called **zero-free** if _none_ of its digits are 0.

Return a string representing the **smallest** **zero-free** number greater than or equal to `num` such that the **product of its digits** is divisible by `t`. If no such number exists, return `"-1"`.

**Example 1:**

**Input:** num = "1234", t = 256

**Output:** "1488"

**Explanation:**

The smallest zero-free number that is greater than 1234 and has the product of its digits divisible by 256 is 1488, with the product of its digits equal to 256.

**Example 2:**

**Input:** num = "12355", t = 50

**Output:** "12355"

**Explanation:**

12355 is already zero-free and has the product of its digits divisible by 50, with the product of its digits equal to 150.

**Example 3:**

**Input:** num = "11111", t = 26

**Output:** "-1"

**Explanation:**

No number greater than 11111 has the product of its digits divisible by 26.

**Constraints:**

*   <code>2 <= num.length <= 2 * 10<sup>5</sup></code>
*   `num` consists only of digits in the range `['0', '9']`.
*   `num` does not contain leading zeros.
*   <code>1 <= t <= 10<sup>14</sup></code>

## Solution

```kotlin
class Solution {
    fun smallestNumber(num: String, t: Long): String {
        var t = t
        var tmp = t
        for (i in 9 downTo 2) {
            while (tmp % i == 0L) {
                tmp /= i.toLong()
            }
        }
        if (tmp > 1) {
            return "-1"
        }
        val s = num.toCharArray()
        val n = s.size
        val leftT = LongArray(n + 1)
        leftT[0] = t
        var i0 = n - 1
        for (i in 0 until n) {
            if (s[i] == '0') {
                i0 = i
                break
            }
            leftT[i + 1] = leftT[i] / gcd(leftT[i], s[i].code.toLong() - '0'.code.toLong())
        }
        if (leftT[n] == 1L) {
            return num
        }
        for (i in i0 downTo 0) {
            while (++s[i] <= '9') {
                var tt = leftT[i] / gcd(leftT[i], s[i].code.toLong() - '0'.code.toLong())
                for (j in n - 1 downTo i + 1) {
                    if (tt == 1L) {
                        s[j] = '1'
                        continue
                    }
                    for (k in 9 downTo 2) {
                        if (tt % k == 0L) {
                            s[j] = ('0'.code + k).toChar()
                            tt /= k.toLong()
                            break
                        }
                    }
                }
                if (tt == 1L) {
                    return String(s)
                }
            }
        }
        val ans = StringBuilder()
        for (i in 9 downTo 2) {
            while (t % i == 0L) {
                ans.append(('0'.code + i).toChar())
                t /= i.toLong()
            }
        }
        while (ans.length <= n) {
            ans.append('1')
        }
        return ans.reverse().toString()
    }

    private fun gcd(a: Long, b: Long): Long {
        var a = a
        var b = b
        while (a != 0L) {
            val tmp = a
            a = b % a
            b = tmp
        }
        return b
    }
}
```