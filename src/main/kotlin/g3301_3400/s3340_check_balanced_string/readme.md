[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3340\. Check Balanced String

Easy

You are given a string `num` consisting of only digits. A string of digits is called **balanced** if the sum of the digits at even indices is equal to the sum of digits at odd indices.

Return `true` if `num` is **balanced**, otherwise return `false`.

**Example 1:**

**Input:** num = "1234"

**Output:** false

**Explanation:**

*   The sum of digits at even indices is `1 + 3 == 4`, and the sum of digits at odd indices is `2 + 4 == 6`.
*   Since 4 is not equal to 6, `num` is not balanced.

**Example 2:**

**Input:** num = "24123"

**Output:** true

**Explanation:**

*   The sum of digits at even indices is `2 + 1 + 3 == 6`, and the sum of digits at odd indices is `4 + 2 == 6`.
*   Since both are equal the `num` is balanced.

**Constraints:**

*   `2 <= num.length <= 100`
*   `num` consists of digits only

## Solution

```kotlin
class Solution {
    fun isBalanced(num: String): Boolean {
        var diff = 0
        var sign = 1
        val n = num.length
        for (i in 0 until n) {
            diff += sign * (num[i].code - '0'.code)
            sign = -sign
        }
        return diff == 0
    }
}
```