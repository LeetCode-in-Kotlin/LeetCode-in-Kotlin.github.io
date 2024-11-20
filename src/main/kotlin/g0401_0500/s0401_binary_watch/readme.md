[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 401\. Binary Watch

Easy

A binary watch has 4 LEDs on the top to represent the hours (0-11), and 6 LEDs on the bottom to represent the minutes (0-59). Each LED represents a zero or one, with the least significant bit on the right.

*   For example, the below binary watch reads `"4:51"`.

![](https://assets.leetcode.com/uploads/2021/04/08/binarywatch.jpg)

Given an integer `turnedOn` which represents the number of LEDs that are currently on (ignoring the PM), return _all possible times the watch could represent_. You may return the answer in **any order**.

The hour must not contain a leading zero.

*   For example, `"01:00"` is not valid. It should be `"1:00"`.

The minute must be consist of two digits and may contain a leading zero.

*   For example, `"10:2"` is not valid. It should be `"10:02"`.

**Example 1:**

**Input:** turnedOn = 1

**Output:** ["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]

**Example 2:**

**Input:** turnedOn = 9

**Output:** []

**Constraints:**

*   `0 <= turnedOn <= 10`

## Solution

```kotlin
import java.util.ArrayList

@Suppress("NAME_SHADOWING")
class Solution {
    fun readBinaryWatch(turnedOn: Int): List<String> {
        val times: MutableList<String> = ArrayList()
        for (hour in 0..11) {
            for (minutes in 0..59) {
                readBinaryWatchHelper(turnedOn, times, hour, minutes)
            }
        }
        return times
    }

    private fun readBinaryWatchHelper(
        turnedOn: Int,
        selectedTimes: MutableList<String>,
        hour: Int,
        minutes: Int,
    ) {
        if (isValidTime(turnedOn, hour, minutes)) {
            selectedTimes.add(getTimeString(hour, minutes))
        }
    }

    private fun getTimeString(hour: Int, minutes: Int): String {
        val time = StringBuilder()
        time.append(hour)
        time.append(':')
        if (minutes < 10) {
            time.append('0')
        }
        time.append(minutes)
        return time.toString()
    }

    private fun isValidTime(turnedOn: Int, hour: Int, minutes: Int): Boolean {
        var hour = hour
        var minutes = minutes
        var counter = 0
        while (hour != 0) {
            if (hour and 1 == 1) {
                counter++
            }
            hour = hour ushr 1
        }
        if (counter > turnedOn) {
            return false
        }
        while (minutes != 0) {
            if (minutes and 1 == 1) {
                counter++
            }
            minutes = minutes ushr 1
        }
        return counter == turnedOn
    }
}
```