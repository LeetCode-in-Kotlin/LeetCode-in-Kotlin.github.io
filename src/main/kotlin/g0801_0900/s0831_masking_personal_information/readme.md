[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 831\. Masking Personal Information

Medium

You are given a personal information string `s`, representing either an **email address** or a **phone number**. Return _the **masked** personal information using the below rules_.

**Email address:**

An email address is:

*   A **name** consisting of uppercase and lowercase English letters, followed by
*   The `'@'` symbol, followed by
*   The **domain** consisting of uppercase and lowercase English letters with a dot `'.'` somewhere in the middle (not the first or last character).

To mask an email:

*   The uppercase letters in the **name** and **domain** must be converted to lowercase letters.
*   The middle letters of the **name** (i.e., all but the first and last letters) must be replaced by 5 asterisks `"*****"`.

**Phone number:**

A phone number is formatted as follows:

*   The phone number contains 10-13 digits.
*   The last 10 digits make up the **local number**.
*   The remaining 0-3 digits, in the beginning, make up the **country code**.
*   **Separation characters** from the set `{'+', '-', '(', ')', ' '}` separate the above digits in some way.

To mask a phone number:

*   Remove all **separation characters**.
*   The masked phone number should have the form:
    *   `"***-***-XXXX"` if the country code has 0 digits.
    *   `"+*-***-***-XXXX"` if the country code has 1 digit.
    *   `"+**-***-***-XXXX"` if the country code has 2 digits.
    *   `"+***-***-***-XXXX"` if the country code has 3 digits.
*   `"XXXX"` is the last 4 digits of the **local number**.

**Example 1:**

**Input:** s = "LeetCode@LeetCode.com"

**Output:** "l\*\*\*\*\*e@leetcode.com"

**Explanation:** s is an email address.

The name and domain are converted to lowercase, and the middle of the name is replaced by 5 asterisks.

**Example 2:**

**Input:** s = "AB@qq.com"

**Output:** "a\*\*\*\*\*b@qq.com"

**Explanation:** s is an email address.

The name and domain are converted to lowercase, and the middle of the name is replaced by 5 asterisks.

Note that even though "ab" is 2 characters, it still must have 5 asterisks in the middle.

**Example 3:**

**Input:** s = "1(234)567-890"

**Output:** "\*\*\*-\*\*\*-7890"

**Explanation:** s is a phone number.

There are 10 digits, so the local number is 10 digits and the country code is 0 digits.

Thus, the resulting masked number is "\*\*\*-\*\*\*-7890".

**Constraints:**

*   `s` is either a **valid** email or a phone number.
*   If `s` is an email:
    *   `8 <= s.length <= 40`
    *   `s` consists of uppercase and lowercase English letters and exactly one `'@'` symbol and `'.'` symbol.
*   If `s` is a phone number:
    *   `10 <= s.length <= 20`
    *   `s` consists of digits, spaces, and the symbols `'('`, `')'`, `'-'`, and `'+'`.

## Solution

```kotlin
class Solution {
    fun maskPII(s: String): String {
        val masked = StringBuilder()
        return if (Character.isAlphabetic(s[0].code)) {
            val locationOfAtSymbol = s.indexOf("@") - 1
            masked.append(s[0]).append("*****").append(s.substring(locationOfAtSymbol))
            masked.toString().lowercase()
        } else {
            val allDigits = StringBuilder()
            var pointer = -1
            while (++pointer < s.length) {
                if (Character.isDigit(s[pointer])) {
                    allDigits.append(s[pointer])
                }
            }
            val numDigits = allDigits.length
            if (numDigits == 11) {
                masked.append("+*-")
            } else if (numDigits == 12) {
                masked.append("+**-")
            } else if (numDigits == 13) {
                masked.append("+***-")
            }
            masked.append("***-***-").append(allDigits.substring(numDigits - 4))
            masked.toString()
        }
    }
}
```