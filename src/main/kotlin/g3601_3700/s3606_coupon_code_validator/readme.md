[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3606\. Coupon Code Validator

Easy

You are given three arrays of length `n` that describe the properties of `n` coupons: `code`, `businessLine`, and `isActive`. The <code>i<sup>th</sup></code> coupon has:

*   `code[i]`: a **string** representing the coupon identifier.
*   `businessLine[i]`: a **string** denoting the business category of the coupon.
*   `isActive[i]`: a **boolean** indicating whether the coupon is currently active.

A coupon is considered **valid** if all of the following conditions hold:

1.  `code[i]` is non-empty and consists only of alphanumeric characters (a-z, A-Z, 0-9) and underscores (`_`).
2.  `businessLine[i]` is one of the following four categories: `"electronics"`, `"grocery"`, `"pharmacy"`, `"restaurant"`.
3.  `isActive[i]` is **true**.

Return an array of the **codes** of all valid coupons, **sorted** first by their **businessLine** in the order: `"electronics"`, `"grocery"`, `"pharmacy", "restaurant"`, and then by **code** in lexicographical (ascending) order within each category.

**Example 1:**

**Input:** code = ["SAVE20","","PHARMA5","SAVE@20"], businessLine = ["restaurant","grocery","pharmacy","restaurant"], isActive = [true,true,true,true]

**Output:** ["PHARMA5","SAVE20"]

**Explanation:**

*   First coupon is valid.
*   Second coupon has empty code (invalid).
*   Third coupon is valid.
*   Fourth coupon has special character `@` (invalid).

**Example 2:**

**Input:** code = ["GROCERY15","ELECTRONICS\_50","DISCOUNT10"], businessLine = ["grocery","electronics","invalid"], isActive = [false,true,true]

**Output:** ["ELECTRONICS\_50"]

**Explanation:**

*   First coupon is inactive (invalid).
*   Second coupon is valid.
*   Third coupon has invalid business line (invalid).

**Constraints:**

*   `n == code.length == businessLine.length == isActive.length`
*   `1 <= n <= 100`
*   `0 <= code[i].length, businessLine[i].length <= 100`
*   `code[i]` and `businessLine[i]` consist of printable ASCII characters.
*   `isActive[i]` is either `true` or `false`.

## Solution

```kotlin
class Solution {
    fun validateCoupons(code: Array<String>, businessLine: Array<String>, isActive: BooleanArray): List<String> {
        val validBusinessLines = hashSetOf("electronics", "grocery", "pharmacy", "restaurant")
        val filteredCoupons = mutableListOf<Pair<String, String>>()
        for (i in code.indices) {
            if (!isActive[i]) {
                continue
            }
            val currentBusinessLine = businessLine[i]
            if (currentBusinessLine !in validBusinessLines) {
                continue
            }
            val currentCode = code[i]
            if (currentCode.isEmpty()) {
                continue
            }

            var isValidCodeChar = true
            for (char in currentCode) {
                if (!(char == '_' || char.isLetterOrDigit())) {
                    isValidCodeChar = false
                    break
                }
            }

            if (isValidCodeChar) {
                filteredCoupons.add(Pair(currentCode, currentBusinessLine))
            }
        }
        filteredCoupons.sortWith(compareBy<Pair<String, String>> { it.second }.thenBy { it.first })
        return filteredCoupons.map { it.first }
    }
}
```