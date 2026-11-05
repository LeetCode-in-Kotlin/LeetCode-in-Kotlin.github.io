[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3986\. Number of Elapsed Seconds Between Two Times

Easy

You are given two valid times `startTime` and `endTime`, each represented as a string in the format `"HH:MM:SS"`.

Return the number of seconds that have elapsed from `startTime` to `endTime`.

**Example 1:**

**Input:** startTime = "01:00:00", endTime = "01:00:25"

**Output:** 25

**Explanation:**

`endTime` is 25 seconds ahead of `startTime`.

**Example 2:**

**Input:** startTime = "12:34:56", endTime = "13:00:00"

**Output:** 1504

**Explanation:**

`endTime` is 25 minutes and 4 seconds ahead of `startTime`, which equals 1504 seconds.

**Constraints:**

*   `startTime.length == 8`
*   `endTime.length == 8`
*   `startTime` and `endTime` are valid times in the format `"HH:MM:SS"`
*   `00 <= HH <= 23`
*   `00 <= MM <= 59`
*   `00 <= SS <= 59`
*   `endTime` is not earlier than `startTime`

## Solution

```kotlin
import kotlin.math.pow

class Solution {
    fun secondsBetweenTimes(startTime: String, endTime: String): Int {
        var startSec = 0
        var endSec = 0
        var sSec = 0
        var eSec = 0
        var colon = 2
        for (i in 0..7) {
            if (startTime[i] == ':') {
                startSec = (startSec + sSec * 60.0.pow(colon.toDouble())).toInt()
                endSec = (endSec + eSec * 60.0.pow(colon.toDouble())).toInt()
                colon--
                sSec = 0
                eSec = 0
                continue
            }
            sSec = sSec * 10 + (startTime[i].code - '0'.code)
            eSec = eSec * 10 + (endTime[i].code - '0'.code)
        }
        startSec = (startSec + sSec * 60.0.pow(colon.toDouble())).toInt()
        endSec = (endSec + eSec * 60.0.pow(colon.toDouble())).toInt()
        return endSec - startSec
    }
}
```