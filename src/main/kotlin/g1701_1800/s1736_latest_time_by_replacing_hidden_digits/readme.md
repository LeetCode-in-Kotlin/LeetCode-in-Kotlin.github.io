[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1736\. Latest Time by Replacing Hidden Digits

Easy

You are given a string `time` in the form of `hh:mm`, where some of the digits in the string are hidden (represented by `?`).

The valid times are those inclusively between `00:00` and `23:59`.

Return _the latest valid time you can get from_ `time` _by replacing the hidden_ _digits_.

**Example 1:**

**Input:** time = "2?:?0"

**Output:** "23:50"

**Explanation:** The latest hour beginning with the digit '2' is 23 and the latest minute ending with the digit '0' is 50.

**Example 2:**

**Input:** time = "0?:3?"

**Output:** "09:39"

**Example 3:**

**Input:** time = "1?:22"

**Output:** "19:22"

**Constraints:**

*   `time` is in the format `hh:mm`.
*   It is guaranteed that you can produce a valid time from the given string.

## Solution

```kotlin
class Solution {
    fun maximumTime(time: String): String {
        val sb: StringBuilder = StringBuilder()
        val strs: Array<String> = time.split(":").dropLastWhile({ it.isEmpty() }).toTypedArray()
        val hour: String = strs[0]
        val min: String = strs[1]
        if (hour[0] == '?') {
            if (hour[1] == '?') {
                sb.append("23")
            } else if (hour[1] > '3') {
                sb.append("1")
                sb.append(hour[1])
            } else {
                sb.append("2")
                sb.append(hour[1])
            }
        } else if (hour[0] == '0' || hour[0] == '1') {
            if (hour[1] == '?') {
                sb.append(hour[0])
                sb.append("9")
            } else {
                sb.append(hour)
            }
        } else if (hour[0] == '2') {
            if (hour[1] == '?') {
                sb.append("23")
            } else {
                sb.append(hour)
            }
        }
        sb.append(":")
        if (min[0] == '?') {
            if (min[1] == '?') {
                sb.append("59")
            } else {
                sb.append("5")
                sb.append(min[1])
            }
            return sb.toString()
        }
        sb.append(min[0])
        if (min[1] == '?') {
            sb.append("9")
        } else {
            sb.append(min[1])
        }
        return sb.toString()
    }
}
```