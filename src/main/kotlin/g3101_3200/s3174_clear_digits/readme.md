[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3174\. Clear Digits

Easy

You are given a string `s`.

Your task is to remove **all** digits by doing this operation repeatedly:

*   Delete the _first_ digit and the **closest** **non-digit** character to its _left_.

Return the resulting string after removing all digits.

**Example 1:**

**Input:** s = "abc"

**Output:** "abc"

**Explanation:**

There is no digit in the string.

**Example 2:**

**Input:** s = "cb34"

**Output:** ""

**Explanation:**

First, we apply the operation on `s[2]`, and `s` becomes `"c4"`.

Then we apply the operation on `s[1]`, and `s` becomes `""`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters and digits.
*   The input is generated such that it is possible to delete all digits.

## Solution

```kotlin
class Solution {
    fun clearDigits(s: String): String {
        val result = StringBuilder()
        for (ch in s.toCharArray()) {
            if (ch in '0'..'9') {
                if (result.isNotEmpty()) {
                    result.deleteCharAt(result.length - 1)
                }
            } else {
                result.append(ch)
            }
        }
        return result.toString()
    }
}
```