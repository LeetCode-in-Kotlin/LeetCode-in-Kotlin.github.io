[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 917\. Reverse Only Letters

Easy

Given a string `s`, reverse the string according to the following rules:

*   All the characters that are not English letters remain in the same position.
*   All the English letters (lowercase or uppercase) should be reversed.

Return `s` _after reversing it_.

**Example 1:**

**Input:** s = "ab-cd"

**Output:** "dc-ba"

**Example 2:**

**Input:** s = "a-bC-dEf-ghIj"

**Output:** "j-Ih-gfE-dCba"

**Example 3:**

**Input:** s = "Test1ng-Leet=code-Q!"

**Output:** "Qedo1ct-eeLg=ntse-T!"

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of characters with ASCII values in the range `[33, 122]`.
*   `s` does not contain `'\"'` or `'\\'`.

## Solution

```kotlin
class Solution {
    fun reverseOnlyLetters(s: String): String {
        val array = s.toCharArray()
        var i = 0
        var j = array.size - 1
        while (i < j) {
            if (Character.isLetter(array[i]) && Character.isLetter(array[j])) {
                val temp = array[i]
                array[i++] = array[j]
                array[j--] = temp
            } else if (Character.isLetter(array[i])) {
                j--
            } else if (Character.isLetter(array[j])) {
                i++
            } else {
                i++
                j--
            }
        }
        return String(array)
    }
}
```