[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2259\. Remove Digit From Number to Maximize Result

Easy

You are given a string `number` representing a **positive integer** and a character `digit`.

Return _the resulting string after removing **exactly one occurrence** of_ `digit` _from_ `number` _such that the value of the resulting string in **decimal** form is **maximized**_. The test cases are generated such that `digit` occurs at least once in `number`.

**Example 1:**

**Input:** number = "123", digit = "3"

**Output:** "12"

**Explanation:** There is only one '3' in "123". After removing '3', the result is "12".

**Example 2:**

**Input:** number = "1231", digit = "1"

**Output:** "231"

**Explanation:** We can remove the first '1' to get "231" or remove the second '1' to get "123".

Since 231 > 123, we return "231".

**Example 3:**

**Input:** number = "551", digit = "5"

**Output:** "51"

**Explanation:** We can remove either the first or second '5' from "551".

Both result in the string "51".

**Constraints:**

*   `2 <= number.length <= 100`
*   `number` consists of digits from `'1'` to `'9'`.
*   `digit` is a digit from `'1'` to `'9'`.
*   `digit` occurs at least once in `number`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun removeDigit(number: String, digit: Char): String {
        var number = number
        var index = 0
        val n = number.length
        for (i in 0 until n) {
            if (number[i] == digit) {
                index = i
                if (i < n - 1 && digit < number[i + 1]) {
                    break
                }
            }
        }
        number = number.substring(0, index) + number.substring(index + 1)
        return number
    }
}
```