[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3556\. Sum of Largest Prime Substrings

Medium

Given a string `s`, find the sum of the **3 largest unique prime numbers** that can be formed using any of its ****substring****.

Return the **sum** of the three largest unique prime numbers that can be formed. If fewer than three exist, return the sum of **all** available primes. If no prime numbers can be formed, return 0.

**Note:** Each prime number should be counted only **once**, even if it appears in **multiple** substrings. Additionally, when converting a substring to an integer, any leading zeros are ignored.

**Example 1:**

**Input:** s = "12234"

**Output:** 1469

**Explanation:**

*   The unique prime numbers formed from the substrings of `"12234"` are 2, 3, 23, 223, and 1223.
*   The 3 largest primes are 1223, 223, and 23. Their sum is 1469.

**Example 2:**

**Input:** s = "111"

**Output:** 11

**Explanation:**

*   The unique prime number formed from the substrings of `"111"` is 11.
*   Since there is only one prime number, the sum is 11.

**Constraints:**

*   `1 <= s.length <= 10`
*   `s` consists of only digits.

## Solution

```kotlin
class Solution {
    fun sumOfLargestPrimes(s: String): Long {
        val set: MutableSet<Long> = HashSet()
        val n = s.length
        var first: Long = -1
        var second: Long = -1
        var third: Long = -1
        for (i in 0..<n) {
            var num: Long = 0
            for (j in i..<n) {
                num = num * 10 + (s[j].code - '0'.code)
                if (i != j && s[i] == '0') {
                    break
                }
                if (isPrime(num) && !set.contains(num)) {
                    set.add(num)
                    if (num > first) {
                        third = second
                        second = first
                        first = num
                    } else if (num > second) {
                        third = second
                        second = num
                    } else if (num > third) {
                        third = num
                    }
                }
            }
        }
        var sum: Long = 0
        if (first != -1L) {
            sum += first
        }
        if (second != -1L) {
            sum += second
        }
        if (third != -1L) {
            sum += third
        }
        return sum
    }

    fun isPrime(num: Long): Boolean {
        if (num <= 1) {
            return false
        }
        if (num == 2L || num == 3L) {
            return true
        }
        if (num % 2 == 0L || num % 3 == 0L) {
            return false
        }
        var i: Long = 5
        while (i * i <= num) {
            if (num % i == 0L || num % (i + 2) == 0L) {
                return false
            }
            i += 6
        }
        return true
    }
}
```