[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 166\. Fraction to Recurring Decimal

Medium

Given two integers representing the `numerator` and `denominator` of a fraction, return _the fraction in string format_.

If the fractional part is repeating, enclose the repeating part in parentheses.

If multiple answers are possible, return **any of them**.

It is **guaranteed** that the length of the answer string is less than <code>10<sup>4</sup></code> for all the given inputs.

**Example 1:**

**Input:** numerator = 1, denominator = 2

**Output:** "0.5"

**Example 2:**

**Input:** numerator = 2, denominator = 1

**Output:** "2"

**Example 3:**

**Input:** numerator = 4, denominator = 333

**Output:** "0.(012)"

**Constraints:**

*   <code>-2<sup>31</sup> <= numerator, denominator <= 2<sup>31</sup> - 1</code>
*   `denominator != 0`

## Solution

```kotlin
class Solution {
    fun fractionToDecimal(numerator: Int, denominator: Int): String {
        if (numerator == 0) {
            return "0"
        }
        val sb = StringBuilder()
        // negative case
        if (numerator > 0 && denominator < 0 || numerator < 0 && denominator > 0) {
            sb.append("-")
        }
        val x = Math.abs(java.lang.Long.valueOf(numerator.toLong()))
        val y = Math.abs(java.lang.Long.valueOf(denominator.toLong()))
        sb.append(x / y)
        var remainder = x % y
        if (remainder == 0L) {
            return sb.toString()
        }
        // decimal case
        sb.append(".")
        // store the remainder in a Hashmap because in the case of recurring decimal, the remainder
        // repeats as dividend.
        val map: MutableMap<Long, Int> = HashMap()
        while (remainder != 0L) {
            if (map.containsKey(remainder)) {
                sb.insert(map.getValue(remainder), "(")
                sb.append(")")
                break
            }
            // store the remainder and the index of it's occurence in the String
            map[remainder] = sb.length
            remainder *= 10
            sb.append(remainder / y)
            remainder %= y
        }
        return sb.toString()
    }
}
```