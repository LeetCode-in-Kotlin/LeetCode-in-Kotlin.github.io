[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 125\. Valid Palindrome

Easy

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"

**Output:** true

**Explanation:** "amanaplanacanalpanama" is a palindrome.

**Example 2:**

**Input:** s = "race a car"

**Output:** false

**Explanation:** "raceacar" is not a palindrome.

**Example 3:**

**Input:** s = " "

**Output:** true

**Explanation:** s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.

**Constraints:**

*   <code>1 <= s.length <= 2 * 10<sup>5</sup></code>
*   `s` consists only of printable ASCII characters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun isPalindrome(s: String): Boolean {
        var i = 0
        var j = s.length - 1
        var res = true
        while (res) {
            // Iterates through string to find first char which is alphanumeric.
            // Done to ignore non-alphanumeric characters.
            // Starts from 0 to j-1.
            while (i < j && isNotAlphaNumeric(s[i])) {
                i++
            }
            // Similarly from j-1 to 0.
            while (i < j && isNotAlphaNumeric(s[j])) {
                j--
            }
            // Checks if i is greater than or equal to j.
            // The main loop only needs to loop n / 2 times hence this condition (where n is string
            // length).
            if (i >= j) {
                break
            }
            // Assigning found indices to variables.
            // The upperToLower function is used to convert characters, if upper case, to lower
            // case.
            // If already lower case, it'll return as it is.
            val left = upperToLower(s[i])
            val right = upperToLower(s[j])
            // If both variables are not same, result becomes false, and breaks out of the loop at
            // next iteration.
            if (left != right) {
                res = false
            }
            i++
            j--
        }
        return res
    }

    private fun isNotAlphaNumeric(c: Char): Boolean {
        return (c < 'a' || c > 'z') && (c < 'A' || c > 'Z') && (c < '0' || c > '9')
    }

    private fun isUpper(c: Char): Boolean {
        return c >= 'A' && c <= 'Z'
    }

    private fun upperToLower(c: Char): Char {
        var c = c
        if (isUpper(c)) {
            c += 32
        }
        return c
    }
}
```