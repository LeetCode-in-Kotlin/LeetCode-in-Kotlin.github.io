[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1360\. Number of Days Between Two Dates

Easy

Write a program to count the number of days between two dates.

The two dates are given as strings, their format is `YYYY-MM-DD` as shown in the examples.

**Example 1:**

**Input:** date1 = "2019-06-29", date2 = "2019-06-30"

**Output:** 1

**Example 2:**

**Input:** date1 = "2020-01-15", date2 = "2019-12-31"

**Output:** 15

**Constraints:**

*   The given dates are valid dates between the years `1971` and `2100`.

## Solution

```kotlin
class Solution {
    fun daysBetweenDates(date1: String, date2: String): Int {
        val strings1 = date1.split("-").dropLastWhile { it.isEmpty() }.toTypedArray()
        val strings2 = date2.split("-").dropLastWhile { it.isEmpty() }.toTypedArray()
        return Math.abs(
            julianDay(strings1[0].toInt(), strings1[1].toInt(), strings1[2].toInt()) -
                julianDay(strings2[0].toInt(), strings2[1].toInt(), strings2[2].toInt()),
        )
    }

    private fun julianDay(year: Int, month: Int, day: Int): Int {
        val a = (14 - month) / 12
        val y = year + 4800 - a
        val m = month + 12 * a - 3
        return day + (153 * m + 2) / 5 + 365 * y + y / 4 - y / 100 + y / 400 - 32045
    }
}
```