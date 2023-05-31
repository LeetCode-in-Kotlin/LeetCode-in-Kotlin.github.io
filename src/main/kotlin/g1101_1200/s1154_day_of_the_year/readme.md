[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1154\. Day of the Year

Easy

Given a string `date` representing a [Gregorian calendar](https://en.wikipedia.org/wiki/Gregorian_calendar) date formatted as `YYYY-MM-DD`, return _the day number of the year_.

**Example 1:**

**Input:** date = "2019-01-09"

**Output:** 9

**Explanation:** Given date is the 9th day of the year in 2019.

**Example 2:**

**Input:** date = "2019-02-10"

**Output:** 41

**Constraints:**

*   `date.length == 10`
*   `date[4] == date[7] == '-'`, and all other `date[i]`'s are digits
*   `date` represents a calendar date between Jan 1<sup>st</sup>, 1900 and Dec 31<sup>th</sup>, 2019.

## Solution

```kotlin
class Solution {
    fun dayOfYear(date: String): Int {
        val monthDays = intArrayOf(0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        val dateArr = date.split("-".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        val year = dateArr[0].toInt()
        val month = dateArr[1].toInt()
        val day = dateArr[2].toInt()
        var dayCount = 0
        val leapYear = year % 4 == 0 && year % 100 != 0 || year % 400 == 0
        for (i in 1 until month) {
            dayCount += monthDays[i]
        }
        dayCount += day
        if (leapYear && month > 2) {
            dayCount++
        }
        return dayCount
    }
}
```