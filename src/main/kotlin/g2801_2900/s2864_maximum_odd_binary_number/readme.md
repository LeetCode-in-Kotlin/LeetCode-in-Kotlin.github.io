[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2864\. Maximum Odd Binary Number

Easy

You are given a **binary** string `s` that contains at least one `'1'`.

You have to **rearrange** the bits in such a way that the resulting binary number is the **maximum odd binary number** that can be created from this combination.

Return _a string representing the maximum odd binary number that can be created from the given combination._

**Note** that the resulting string **can** have leading zeros.

**Example 1:**

**Input:** s = "010"

**Output:** "001"

**Explanation:** Because there is just one '1', it must be in the last position. So the answer is "001".

**Example 2:**

**Input:** s = "0101"

**Output:** "1001"

**Explanation:** One of the '1's must be in the last position. The maximum number that can be made with the remaining digits is "100". So the answer is "1001".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of `'0'` and `'1'`.
*   `s` contains at least one `'1'`.

## Solution

```kotlin
class Solution {
    fun maximumOddBinaryNumber(s: String): String {
        var len = s.length
        var count = 0
        val sb = StringBuilder()
        for (i in 0 until len) {
            if (s[i] == '1') {
                count++
            }
        }
        if (count == len) {
            return s
        }
        len -= count
        while (count > 1) {
            sb.append('1')
            count--
        }
        while (len > 0) {
            sb.append('0')
            len--
        }
        sb.append('1')
        return sb.toString()
    }
}
```