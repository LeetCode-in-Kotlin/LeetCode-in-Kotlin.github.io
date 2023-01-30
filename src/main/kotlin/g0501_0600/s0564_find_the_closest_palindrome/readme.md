[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 564\. Find the Closest Palindrome

Hard

Given a string `n` representing an integer, return _the closest integer (not including itself), which is a palindrome_. If there is a tie, return _**the smaller one**_.

The closest is defined as the absolute difference minimized between two integers.

**Example 1:**

**Input:** n = "123"

**Output:** "121"

**Example 2:**

**Input:** n = "1"

**Output:** "0"

**Explanation:** 0 and 2 are the closest palindromes but we return the smallest which is 0.

**Constraints:**

*   `1 <= n.length <= 18`
*   `n` consists of only digits.
*   `n` does not have leading zeros.
*   `n` is representing an integer in the range <code>[1, 10<sup>18</sup> - 1]</code>.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun nearestPalindromic(n: String): String {
        if (n.length == 1) {
            return (n.toInt() - 1).toString()
        }
        val num = n.toLong()
        val offset = Math.pow(10.0, (n.length / 2).toDouble()).toInt()
        val first =
            if (isPalindrome(n)) palindromeGenerator(num + offset, n.length) else palindromeGenerator(num, n.length)
        val second = if (first < num) palindromeGenerator(num + offset, n.length) else palindromeGenerator(
            num - offset,
            n.length
        )
        if (first + second == 2 * num) {
            return if (first < second) first.toString() else second.toString()
        }
        return if (Math.abs(num - first) > Math.abs(num - second)) second.toString() else first.toString()
    }

    private fun palindromeGenerator(num: Long, length: Int): Long {
        var num = num
        if (num < 10) {
            return 9
        }
        val numOfDigits = num.toString().length
        if (numOfDigits > length) {
            return Math.pow(10.0, (numOfDigits - 1).toDouble()).toLong() + 1
        } else if (numOfDigits < length) {
            return Math.pow(10.0, numOfDigits.toDouble()).toLong() - 1
        }
        num = num - num % Math.pow(10.0, (numOfDigits / 2).toDouble()).toLong()
        var temp = num
        for (j in 0 until numOfDigits / 2) {
            val digit = Math.pow(10.0, (numOfDigits - j - 1).toDouble()).toLong()
            num += (temp / digit * Math.pow(10.0, j.toDouble())).toInt().toLong()
            temp = temp % digit
        }
        return num
    }

    private fun isPalindrome(str: String): Boolean {
        for (i in 0 until str.length / 2) {
            if (str[i] != str[str.length - 1 - i]) {
                return false
            }
        }
        return true
    }
}
```