[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3114\. Latest Time You Can Obtain After Replacing Characters

Easy

You are given a string `s` representing a 12-hour format time where some of the digits (possibly none) are replaced with a `"?"`.

12-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `11`, and `MM` is between `00` and `59`. The earliest 12-hour time is `00:00`, and the latest is `11:59`.

You have to replace **all** the `"?"` characters in `s` with digits such that the time we obtain by the resulting string is a **valid** 12-hour format time and is the **latest** possible.

Return _the resulting string_.

**Example 1:**

**Input:** s = "1?:?4"

**Output:** "11:54"

**Explanation:** The latest 12-hour format time we can achieve by replacing `"?"` characters is `"11:54"`.

**Example 2:**

**Input:** s = "0?:5?"

**Output:** "09:59"

**Explanation:** The latest 12-hour format time we can achieve by replacing `"?"` characters is `"09:59"`.

**Constraints:**

*   `s.length == 5`
*   `s[2]` is equal to the character `":"`.
*   All characters except `s[2]` are digits or `"?"` characters.
*   The input is generated such that there is **at least** one time between `"00:00"` and `"11:59"` that you can obtain after replacing the `"?"` characters.

## Solution

```kotlin
class Solution {
    fun findLatestTime(s: String): String {
        val nm = StringBuilder()
        if (s[0] == '?' && s[1] == '?') {
            nm.append("11")
        } else if (s[0] != '?' && s[1] == '?') {
            nm.append(s[0])
            if (s[0] == '1') {
                nm.append("1")
            } else {
                nm.append("9")
            }
        } else if (s[0] == '?' && s[1] != '?') {
            if (s[1] in '2'..'9') {
                nm.append("0")
            } else {
                nm.append("1")
            }
            nm.append(s[1])
        } else {
            nm.append(s[0])
            nm.append(s[1])
        }
        nm.append(":")
        if (s[3] == '?' && s[4] == '?') {
            nm.append("59")
        } else if (s[3] != '?' && s[4] == '?') {
            nm.append(s[3])
            nm.append("9")
        } else if (s[3] == '?' && s[4] != '?') {
            nm.append("5")
            nm.append(s[4])
        } else {
            nm.append(s[3])
            nm.append(s[4])
        }
        return nm.toString()
    }
}
```