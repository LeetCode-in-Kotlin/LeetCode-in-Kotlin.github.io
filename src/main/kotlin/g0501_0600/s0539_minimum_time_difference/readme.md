[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 539\. Minimum Time Difference

Medium

Given a list of 24-hour clock time points in **"HH:MM"** format, return _the minimum **minutes** difference between any two time-points in the list_.

**Example 1:**

**Input:** timePoints = ["23:59","00:00"]

**Output:** 1

**Example 2:**

**Input:** timePoints = ["00:00","23:59","00:00"]

**Output:** 0

**Constraints:**

*   <code>2 <= timePoints.length <= 2 * 10<sup>4</sup></code>
*   `timePoints[i]` is in the format **"HH:MM"**.

## Solution

```kotlin
class Solution {
    fun findMinDifference(timePoints: List<String>): Int {
        return if (timePoints.size < 300) {
            smallInputSize(timePoints)
        } else {
            largeInputSize(timePoints)
        }
    }

    private fun largeInputSize(timePoints: List<String>): Int {
        val times = BooleanArray(60 * 24)
        for (time in timePoints) {
            val hours = time.substring(0, 2).toInt()
            val minutes = time.substring(3, 5).toInt()
            if (times[hours * 60 + minutes]) {
                return 0
            }
            times[hours * 60 + minutes] = true
        }
        var prev = -1
        var min = 60 * 24
        for (i in 0 until times.size + times.size / 2) {
            if (i < times.size) {
                if (times[i] && prev == -1) {
                    prev = i
                } else if (times[i]) {
                    min = Math.min(min, i - prev)
                    prev = i
                }
            } else {
                if (times[i - times.size] && prev == -1) {
                    prev = i
                } else if (times[i - times.size]) {
                    min = Math.min(min, i - prev)
                    prev = i
                }
            }
        }
        return min
    }

    private fun smallInputSize(timePoints: List<String>): Int {
        val times = IntArray(timePoints.size)
        var j = 0
        for (time in timePoints) {
            val hours = time.substring(0, 2).toInt()
            val minutes = time.substring(3, 5).toInt()
            times[j++] = hours * 60 + minutes
        }
        times.sort()
        var min = 60 * 24
        for (i in 1..times.size) {
            min = if (i == times.size) {
                Math.min(min, times[0] + 60 * 24 - times[times.size - 1])
            } else {
                Math.min(min, times[i] - times[i - 1])
            }
            if (min == 0) {
                return 0
            }
        }
        return min
    }
}
```