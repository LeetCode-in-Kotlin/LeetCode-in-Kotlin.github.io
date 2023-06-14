[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1507\. Reformat Date

Easy

Given a `date` string in the form `Day Month Year`, where:

*   `Day` is in the set `{"1st", "2nd", "3rd", "4th", ..., "30th", "31st"}`.
*   `Month` is in the set `{"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"}`.
*   `Year` is in the range `[1900, 2100]`.

Convert the date string to the format `YYYY-MM-DD`, where:

*   `YYYY` denotes the 4 digit year.
*   `MM` denotes the 2 digit month.
*   `DD` denotes the 2 digit day.

**Example 1:**

**Input:** date = "20th Oct 2052"

**Output:** "2052-10-20"

**Example 2:**

**Input:** date = "6th Jun 1933"

**Output:** "1933-06-06"

**Example 3:**

**Input:** date = "26th May 1960"

**Output:** "1960-05-26"

**Constraints:**

*   The given dates are guaranteed to be valid, so no error handling is necessary.

## Solution

```kotlin
class Solution {
    fun reformatDate(date: String): String {
        val sb = StringBuilder()
        val map: MutableMap<String, String> = HashMap()
        map["Jan"] = "01"
        map["Feb"] = "02"
        map["Mar"] = "03"
        map["Apr"] = "04"
        map["May"] = "05"
        map["Jun"] = "06"
        map["Jul"] = "07"
        map["Aug"] = "08"
        map["Sep"] = "09"
        map["Oct"] = "10"
        map["Nov"] = "11"
        map["Dec"] = "12"
        sb.append(date.substring(date.length - 4))
        sb.append('-')
        sb.append(map[date.substring(date.length - 8, date.length - 5)])
        sb.append('-')
        if (Character.isDigit(date[1])) {
            sb.append(date, 0, 2)
        } else {
            sb.append('0')
            sb.append(date[0])
        }
        return sb.toString()
    }
}
```