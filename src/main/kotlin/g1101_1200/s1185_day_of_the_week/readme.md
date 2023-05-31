[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1185\. Day of the Week

Easy

Given a date, return the corresponding day of the week for that date.

The input is given as three integers representing the `day`, `month` and `year` respectively.

Return the answer as one of the following values `{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}`.

**Example 1:**

**Input:** day = 31, month = 8, year = 2019

**Output:** "Saturday"

**Example 2:**

**Input:** day = 18, month = 7, year = 1999

**Output:** "Sunday"

**Example 3:**

**Input:** day = 15, month = 8, year = 1993

**Output:** "Sunday"

**Constraints:**

*   The given dates are valid dates between the years `1971` and `2100`.

## Solution

```kotlin
class Solution {
    fun dayOfTheWeek(day: Int, month: Int, year: Int): String {
        var counter = 0
        for (i in 1971 until year) {
            counter += if (isLeapYear(i)) {
                366
            } else {
                365
            }
        }
        for (i in 1 until month) {
            counter += dayOfMonth(i)
        }
        for (i in 1..day) {
            counter += 1
        }
        if (isLeapYear(year) && month > 2) {
            counter++
        }
        return when (counter % 7) {
            1 -> "Friday"
            2 -> "Saturday"
            3 -> "Sunday"
            4 -> "Monday"
            5 -> "Tuesday"
            6 -> "Wednesday"
            else -> "Thursday"
        }
    }

    private fun isLeapYear(year: Int): Boolean {
        return year % 4 == 0 && year % 100 != 0 || year % 400 == 0
    }

    private fun dayOfMonth(month: Int): Int {
        return when (month) {
            1, 3, 5, 7, 8, 10, 12 -> 31
            4, 6, 9, 11 -> 30
            else -> 28
        }
    }
}
```