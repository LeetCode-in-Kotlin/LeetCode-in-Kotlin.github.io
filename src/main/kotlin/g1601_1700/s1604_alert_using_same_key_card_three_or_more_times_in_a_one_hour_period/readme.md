[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1604\. Alert Using Same Key-Card Three or More Times in a One Hour Period

Medium

LeetCode company workers use key-cards to unlock office doors. Each time a worker uses their key-card, the security system saves the worker's name and the time when it was used. The system emits an **alert** if any worker uses the key-card **three or more times** in a one-hour period.

You are given a list of strings `keyName` and `keyTime` where `[keyName[i], keyTime[i]]` corresponds to a person's name and the time when their key-card was used **in a** **single day**.

Access times are given in the **24-hour time format "HH:MM"**, such as `"23:51"` and `"09:49"`.

Return a _list of unique worker names who received an alert for frequent keycard use_. Sort the names in **ascending order alphabetically**.

Notice that `"10:00"` - `"11:00"` is considered to be within a one-hour period, while `"22:51"` - `"23:52"` is not considered to be within a one-hour period.

**Example 1:**

**Input:** keyName = ["daniel","daniel","daniel","luis","luis","luis","luis"], keyTime = ["10:00","10:40","11:00","09:00","11:00","13:00","15:00"]

**Output:** ["daniel"]

**Explanation:** "daniel" used the keycard 3 times in a one-hour period ("10:00","10:40", "11:00").

**Example 2:**

**Input:** keyName = ["alice","alice","alice","bob","bob","bob","bob"], keyTime = ["12:01","12:00","18:00","21:00","21:20","21:30","23:00"]

**Output:** ["bob"]

**Explanation:** "bob" used the keycard 3 times in a one-hour period ("21:00","21:20", "21:30").

**Constraints:**

*   <code>1 <= keyName.length, keyTime.length <= 10<sup>5</sup></code>
*   `keyName.length == keyTime.length`
*   `keyTime[i]` is in the format **"HH:MM"**.
*   `[keyName[i], keyTime[i]]` is **unique**.
*   `1 <= keyName[i].length <= 10`
*   `keyName[i] contains only lowercase English letters.`

## Solution

```kotlin
class Solution {
    fun alertNames(keyName: Array<String>, keyTime: Array<String>): List<String> {
        val map = HashMap<String, MutableList<String>>()
        for (i in keyName.indices) {
            map.putIfAbsent(keyName[i], ArrayList())
            map[keyName[i]]!!.add(keyTime[i])
        }
        val soln: MutableList<String> = ArrayList()
        for ((key, timeStamps) in map) {
            timeStamps.sort()
            var i = 0
            while (i + 2 < timeStamps.size) {
                val first = timeStamps[i].split(":").dropLastWhile { it.isEmpty() }.toTypedArray()
                val third = timeStamps[i + 2].split(":").dropLastWhile { it.isEmpty() }.toTypedArray()
                val hourDiff = third[0].toInt() - first[0].toInt()
                val minDiff = third[1].toInt() - first[1].toInt()
                if (hourDiff == 0 || hourDiff == 1 && minDiff <= 0) {
                    soln.add(key)
                    break
                }
                i++
            }
        }
        soln.sort()
        return soln
    }
}
```