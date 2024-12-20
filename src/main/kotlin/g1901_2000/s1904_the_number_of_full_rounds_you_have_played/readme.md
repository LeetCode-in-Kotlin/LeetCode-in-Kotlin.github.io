[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1904\. The Number of Full Rounds You Have Played

Medium

You are participating in an online chess tournament. There is a chess round that starts every `15` minutes. The first round of the day starts at `00:00`, and after every `15` minutes, a new round starts.

*   For example, the second round starts at `00:15`, the fourth round starts at `00:45`, and the seventh round starts at `01:30`.

You are given two strings `loginTime` and `logoutTime` where:

*   `loginTime` is the time you will login to the game, and
*   `logoutTime` is the time you will logout from the game.

If `logoutTime` is **earlier** than `loginTime`, this means you have played from `loginTime` to midnight and from midnight to `logoutTime`.

Return _the number of full chess rounds you have played in the tournament_.

**Note:** All the given times follow the 24-hour clock. That means the first round of the day starts at `00:00` and the last round of the day starts at `23:45`.

**Example 1:**

**Input:** loginTime = "09:31", logoutTime = "10:14"

**Output:** 1

**Explanation:** 

You played one full round from 09:45 to 10:00. You did not play the full round from 09:30 to 09:45 because you logged in at 09:31 after it began. 

You did not play the full round from 10:00 to 10:15 because you logged out at 10:14 before it ended.

**Example 2:**

**Input:** loginTime = "21:30", logoutTime = "03:00"

**Output:** 22

**Explanation:** You played 10 full rounds from 21:30 to 00:00 and 12 full rounds from 00:00 to 03:00. 10 + 12 = 22.

**Constraints:**

*   `loginTime` and `logoutTime` are in the format `hh:mm`.
*   `00 <= hh <= 23`
*   `00 <= mm <= 59`
*   `loginTime` and `logoutTime` are not equal.

## Solution

```kotlin
class Solution {
    fun numberOfRounds(loginTime: String, logoutTime: String): Int {
        var loginSerializeTime = serializeTime(loginTime)
        var logoutSerializeTime = serializeTime(logoutTime)
        if (logoutSerializeTime - 14 < loginSerializeTime &&
            logoutSerializeTime > loginSerializeTime
        ) {
            return 0
        }
        loginSerializeTime = maskSerializeTime(loginSerializeTime, 14)
        logoutSerializeTime = maskSerializeTime(logoutSerializeTime, 0)
        if (loginSerializeTime == logoutSerializeTime) {
            return 0
        }
        return if (loginSerializeTime > logoutSerializeTime + 14) {
            (
                calculateFullRounds(loginSerializeTime, MID_NIGHT_END) +
                    calculateFullRounds(MID_NIGHT_START, logoutSerializeTime)
                )
        } else {
            calculateFullRounds(loginSerializeTime, logoutSerializeTime)
        }
    }

    private fun maskSerializeTime(serializeTime: Int, mask: Int): Int {
        return (serializeTime + mask) / ROUND_INTERVAL * ROUND_INTERVAL
    }

    private fun serializeTime(time: String): Int {
        return time.substring(0, 2).toInt() * 60 + time.substring(3, 5).toInt()
    }

    private fun calculateFullRounds(login: Int, logout: Int): Int {
        return (logout - login) / ROUND_INTERVAL
    }

    companion object {
        private const val MID_NIGHT_END = 1440
        private const val MID_NIGHT_START = 0
        private const val ROUND_INTERVAL = 15
    }
}
```