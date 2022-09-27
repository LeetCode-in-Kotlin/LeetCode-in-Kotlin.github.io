[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 93\. Restore IP Addresses

Medium

A **valid IP address** consists of exactly four integers separated by single dots. Each integer is between `0` and `255` (**inclusive**) and cannot have leading zeros.

*   For example, `"0.1.2.201"` and `"192.168.1.1"` are **valid** IP addresses, but `"0.011.255.245"`, `"192.168.1.312"` and `"192.168@1.1"` are **invalid** IP addresses.

Given a string `s` containing only digits, return _all possible valid IP addresses that can be formed by inserting dots into_ `s`. You are **not** allowed to reorder or remove any digits in `s`. You may return the valid IP addresses in **any** order.

**Example 1:**

**Input:** s = "25525511135"

**Output:** ["255.255.11.135","255.255.111.35"]

**Example 2:**

**Input:** s = "0000"

**Output:** ["0.0.0.0"]

**Example 3:**

**Input:** s = "101023"

**Output:** ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]

**Constraints:**

*   `1 <= s.length <= 20`
*   `s` consists of digits only.

## Solution

```kotlin
class Solution() {
    fun restoreIpAddresses(s: String): List<String> {
        val results: MutableList<String> = ArrayList()
        step(s, 0, IntArray(4), 0, results)
        return results
    }

    fun step(s: String, pos: Int, octets: IntArray, count: Int, results: MutableList<String>) {
        if (count == 4 && pos == s.length) {
            results.add(
                octets[0].toString() + '.' +
                    octets[1] +
                    '.' +
                    octets[2] +
                    '.' +
                    octets[3]
            )
        } else if (count < 4 && pos < 12) {
            var octet = 0
            for (i in 0..2) {
                if (pos + i < s.length) {
                    val digit = s[pos + i] - '0'
                    octet = octet * 10 + digit
                    if (octet < 256) {
                        octets[count] = octet
                        step(s, pos + i + 1, octets, count + 1, results)
                    }
                    if (i == 0 && digit == 0) {
                        break
                    }
                }
            }
        }
    }
}
```