[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 402\. Remove K Digits

Medium

Given string num representing a non-negative integer `num`, and an integer `k`, return _the smallest possible integer after removing_ `k` _digits from_ `num`.

**Example 1:**

**Input:** num = "1432219", k = 3

**Output:** "1219"

**Explanation:** Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

**Example 2:**

**Input:** num = "10200", k = 1

**Output:** "200"

**Explanation:** Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

**Example 3:**

**Input:** num = "10", k = 2

**Output:** "0"

**Explanation:** Remove all the digits from the number and it is left with nothing which is 0.

**Constraints:**

*   <code>1 <= k <= num.length <= 10<sup>5</sup></code>
*   `num` consists of only digits.
*   `num` does not have any leading zeros except for the zero itself.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun removeKdigits(num: String, k: Int): String {
        var k = k
        val list = CharArray(num.length)
        val len = num.length - k
        var top = 0
        for (i in 0 until num.length) {
            while (top > 0 && k > 0 && num[i] < list[top - 1]) {
                top--
                k--
            }
            list[top++] = num[i]
        }
        var number = 0
        while (number < len && list[number] == '0') {
            number++
        }
        return if (number == len) "0" else String(list, number, len - number)
    }
}
```