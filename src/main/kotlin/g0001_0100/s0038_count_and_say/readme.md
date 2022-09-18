[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 38\. Count and Say

Medium

The **count-and-say** sequence is a sequence of digit strings defined by the recursive formula:

*   `countAndSay(1) = "1"`
*   `countAndSay(n)` is the way you would "say" the digit string from `countAndSay(n-1)`, which is then converted into a different digit string.

To determine how you "say" a digit string, split it into the **minimal** number of substrings such that each substring contains exactly **one** unique digit. Then for each substring, say the number of digits, then say the digit. Finally, concatenate every said digit.

For example, the saying and conversion for digit string `"3322251"`:

![](https://assets.leetcode.com/uploads/2020/10/23/countandsay.jpg)

Given a positive integer `n`, return _the_ <code>n<sup>th</sup></code> _term of the **count-and-say** sequence_.

**Example 1:**

**Input:** n = 1

**Output:** "1"

**Explanation:** This is the base case.

**Example 2:**

**Input:** n = 4

**Output:** "1211"

**Explanation:** countAndSay(1) = "1" countAndSay(2) = say "1" = one 1 = "11" countAndSay(3) = say "11" = two 1's = "21" countAndSay(4) = say "21" = one 2 + one 1 = "12" + "11" = "1211"

**Constraints:**

*   `1 <= n <= 30`

## Solution

```kotlin
class Solution {
    fun countAndSay(n: Int): String {
        if (n == 1) {
            return "1"
        }
        val res = StringBuilder()
        val prev = countAndSay(n - 1)
        var count = 1
        for (i in 1 until prev.length) {
            if (prev[i] == prev[i - 1]) {
                count++
            } else {
                res.append(count).append(prev[i - 1])
                count = 1
            }
        }
        res.append(count).append(prev[prev.length - 1])
        return res.toString()
    }
}
```