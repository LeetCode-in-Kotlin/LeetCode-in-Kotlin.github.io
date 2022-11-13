[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 306\. Additive Number

Medium

An **additive number** is a string whose digits can form an **additive sequence**.

A valid **additive sequence** should contain **at least** three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits, return `true` if it is an **additive number** or `false` otherwise.

**Note:** Numbers in the additive sequence **cannot** have leading zeros, so sequence `1, 2, 03` or `1, 02, 3` is invalid.

**Example 1:**

**Input:** "112358"

**Output:** true

**Explanation:** The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8

**Example 2:**

**Input:** "199100199"

**Output:** true

**Explanation:** The additive sequence is: 1, 99, 100, 199. 1 + 99 = 100, 99 + 100 = 199

**Constraints:**

*   `1 <= num.length <= 35`
*   `num` consists only of digits.

**Follow up:** How would you handle overflow for very large input integers?

## Solution

```kotlin
class Solution {
    fun isAdditiveNumber(num: String): Boolean {
        if (num.isEmpty() || num.length < 3) {
            return false
        }

        fun isInvalid(s: String): Boolean {
            return s[0] == '0' && s.length > 1
        }

        fun backtrack(first: Long, second: Long, startIndex: Int): Boolean {
            val third = (first + second).toString()
            if (num.substring(startIndex).startsWith(third)) {
                if (third.length == num.length - startIndex) return true
                return backtrack(second, third.toLong(), startIndex + third.length)
            }
            return false
        }

        for (i in 1 until num.length) {
            val first = num.substring(0, i)
            if (isInvalid(first)) break
            for (j in i + 1 until num.length) {
                val second = num.substring(i, j)
                if (isInvalid(second)) break
                if (backtrack(first.toLong(), second.toLong(), j)) return true
            }
        }
        return false
    }
}
```