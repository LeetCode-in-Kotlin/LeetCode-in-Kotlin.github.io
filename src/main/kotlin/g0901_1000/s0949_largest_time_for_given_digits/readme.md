[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 949\. Largest Time for Given Digits

Medium

Given an array `arr` of 4 digits, find the latest 24-hour time that can be made using each digit **exactly once**.

24-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `23`, and `MM` is between `00` and `59`. The earliest 24-hour time is `00:00`, and the latest is `23:59`.

Return _the latest 24-hour time in `"HH:MM"` format_. If no valid time can be made, return an empty string.

**Example 1:**

**Input:** arr = [1,2,3,4]

**Output:** "23:41"

**Explanation:** The valid 24-hour times are "12:34", "12:43", "13:24", "13:42", "14:23", "14:32", "21:34", "21:43", "23:14", and "23:41". Of these times, "23:41" is the latest.

**Example 2:**

**Input:** arr = [5,5,5,5]

**Output:** ""

**Explanation:** There are no valid 24-hour times as "55:55" is not valid.

**Constraints:**

*   `arr.length == 4`
*   `0 <= arr[i] <= 9`

## Solution

```kotlin
class Solution {
    fun largestTimeFromDigits(arr: IntArray): String {
        val buf = StringBuilder()
        var maxHour: String? = ""
        for (i in 0..3) {
            for (j in 0..3) {
                if (i != j) {
                    val hour = getTime(arr, i, j, 23, false)
                    val min = getTime(arr, i, j, 59, true)
                    if (hour != null && min != null && hour > maxHour!!) {
                        buf.setLength(0)
                        buf.append(hour).append(':').append(min)
                        maxHour = hour
                    }
                }
            }
        }
        return buf.toString()
    }

    private fun getTime(arr: IntArray, i: Int, j: Int, limit: Int, exclude: Boolean): String? {
        var n1 = -1
        var n2 = -1
        for (k in 0..3) {
            if (exclude && k != i && k != j || !exclude && (k == i || k == j)) {
                if (n1 == -1) {
                    n1 = arr[k]
                } else {
                    n2 = arr[k]
                }
            }
        }
        var s1: String? = n1.toString() + n2
        var s2: String? = n2.toString() + n1
        var v1 = s1!!.toInt()
        if (v1 > limit) {
            v1 = -1
            s1 = null
        }
        var v2 = s2!!.toInt()
        if (v2 > limit) {
            v2 = -1
            s2 = null
        }
        return if (v1 >= v2) s1 else s2
    }
}
```