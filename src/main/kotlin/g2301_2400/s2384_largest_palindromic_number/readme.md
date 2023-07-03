[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2384\. Largest Palindromic Number

Medium

You are given a string `num` consisting of digits only.

Return _the **largest palindromic** integer (in the form of a string) that can be formed using digits taken from_ `num`. It should not contain **leading zeroes**.

**Notes:**

*   You do **not** need to use all the digits of `num`, but you must use **at least** one digit.
*   The digits can be reordered.

**Example 1:**

**Input:** num = "444947137"

**Output:** "7449447"

**Explanation:**

Use the digits "4449477" from "<ins>**44494**</ins><ins>**7**</ins>13<ins>**7**</ins>" to form the palindromic integer "7449447".

It can be shown that "7449447" is the largest palindromic integer that can be formed.

**Example 2:**

**Input:** num = "00009"

**Output:** "9"

**Explanation:**

It can be shown that "9" is the largest palindromic integer that can be formed.

Note that the integer returned should not contain leading zeroes.

**Constraints:**

*   <code>1 <= num.length <= 10<sup>5</sup></code>
*   `num` consists of digits.

## Solution

```kotlin
class Solution {
    fun largestPalindromic(num: String): String {
        val count = IntArray(10)
        var center = -1
        val first = StringBuilder()
        for (c in num.toCharArray()) {
            count[c.code - '0'.code]++
        }
        var c: Int
        for (i in 9 downTo 0) {
            c = 0
            if (count[i] % 2 == 1 && center == -1) {
                center = i
            }
            if (first.length == 0 && i == 0) {
                continue
            }
            while (c < count[i] / 2) {
                first.append(i.toString())
                c++
            }
        }
        val second: StringBuilder = StringBuilder(first.toString())
        if (center != -1) {
            first.append(center)
        }
        first.append(second.reverse().toString())
        return if (first.length == 0) "0" else first.toString()
    }
}
```