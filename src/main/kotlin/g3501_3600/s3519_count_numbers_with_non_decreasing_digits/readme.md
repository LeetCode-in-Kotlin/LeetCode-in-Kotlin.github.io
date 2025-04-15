[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3519\. Count Numbers with Non-Decreasing Digits

Hard

You are given two integers, `l` and `r`, represented as strings, and an integer `b`. Return the count of integers in the inclusive range `[l, r]` whose digits are in **non-decreasing** order when represented in base `b`.

An integer is considered to have **non-decreasing** digits if, when read from left to right (from the most significant digit to the least significant digit), each digit is greater than or equal to the previous one.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** l = "23", r = "28", b = 8

**Output:** 3

**Explanation:**

*   The numbers from 23 to 28 in base 8 are: 27, 30, 31, 32, 33, and 34.
*   Out of these, 27, 33, and 34 have non-decreasing digits. Hence, the output is 3.

**Example 2:**

**Input:** l = "2", r = "7", b = 2

**Output:** 2

**Explanation:**

*   The numbers from 2 to 7 in base 2 are: 10, 11, 100, 101, 110, and 111.
*   Out of these, 11 and 111 have non-decreasing digits. Hence, the output is 2.

**Constraints:**

*   `1 <= l.length <= r.length <= 100`
*   `2 <= b <= 10`
*   `l` and `r` consist only of digits.
*   The value represented by `l` is less than or equal to the value represented by `r`.
*   `l` and `r` do not contain leading zeros.

## Solution

```kotlin
class Solution {
    fun countNumbers(l: String, r: String, b: Int): Int {
        val ans1 = find(r.toCharArray(), b)
        val start = subTractOne(l.toCharArray())
        val ans2 = find(start, b)
        return ((ans1 - ans2) % 1000000007L).toInt()
    }

    private fun find(arr: CharArray, b: Int): Long {
        val nums = convertNumToBase(arr, b)
        val dp = Array<Array<Array<Long?>>>(nums.size) { Array<Array<Long?>>(2) { arrayOfNulls<Long>(11) } }
        return solve(0, nums, 1, b, 0, dp) - 1
    }

    private fun solve(i: Int, arr: IntArray, tight: Int, base: Int, last: Int, dp: Array<Array<Array<Long?>>>): Long {
        if (i == arr.size) {
            return 1L
        }
        if (dp[i][tight][last] != null) {
            return dp[i][tight][last]!!
        }
        var till = base - 1
        if (tight == 1) {
            till = arr[i]
        }
        var ans: Long = 0
        for (j in 0..till) {
            if (j >= last) {
                ans = (ans + solve(i + 1, arr, tight and (if (j == arr[i]) 1 else 0), base, j, dp))
            }
        }
        dp[i][tight][last] = ans
        return ans
    }

    private fun subTractOne(arr: CharArray): CharArray {
        val n = arr.size
        var i = n - 1
        while (i >= 0 && arr[i] == '0') {
            arr[i--] = '9'
        }
        val x = arr[i].code - '0'.code - 1
        arr[i] = (x + '0'.code).toChar()
        var j = 0
        var idx = 0
        while (j < n && arr[j] == '0') {
            j++
        }
        val res = CharArray(n - j)
        for (k in j..<n) {
            res[idx++] = arr[k]
        }
        return res
    }

    private fun convertNumToBase(arr: CharArray, base: Int): IntArray {
        val n = arr.size
        var num = IntArray(n)
        var i = 0
        while (i < n) {
            num[i] = arr[i++].code - '0'.code
        }
        val temp: MutableList<Int?> = ArrayList<Int?>()
        var len = n
        while (len > 0) {
            var rem = 0
            val next = IntArray(len)
            var newLen = 0
            var j = 0
            while (j < len) {
                val cur = rem * 10L + num[j]
                val q = (cur / base).toInt()
                rem = (cur % base).toInt()
                if (newLen > 0 || q != 0) {
                    next[newLen] = q
                    newLen++
                }
                j++
            }
            temp.add(rem)
            num = next
            len = newLen
        }
        val res = IntArray(temp.size)
        var k = 0
        val size = temp.size
        while (k < size) {
            res[k] = temp[size - 1 - k]!!
            k++
        }
        return res
    }
}
```