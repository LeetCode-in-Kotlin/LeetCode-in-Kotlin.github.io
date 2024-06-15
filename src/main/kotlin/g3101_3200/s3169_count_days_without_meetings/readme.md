[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3169\. Count Days Without Meetings

Medium

You are given a positive integer `days` representing the total number of days an employee is available for work (starting from day 1). You are also given a 2D array `meetings` of size `n` where, `meetings[i] = [start_i, end_i]` represents the starting and ending days of meeting `i` (inclusive).

Return the count of days when the employee is available for work but no meetings are scheduled.

**Note:** The meetings may overlap.

**Example 1:**

**Input:** days = 10, meetings = \[\[5,7],[1,3],[9,10]]

**Output:** 2

**Explanation:**

There is no meeting scheduled on the 4<sup>th</sup> and 8<sup>th</sup> days.

**Example 2:**

**Input:** days = 5, meetings = \[\[2,4],[1,3]]

**Output:** 1

**Explanation:**

There is no meeting scheduled on the 5<sup>th</sup> day.

**Example 3:**

**Input:** days = 6, meetings = \[\[1,6]]

**Output:** 0

**Explanation:**

Meetings are scheduled for all working days.

**Constraints:**

*   <code>1 <= days <= 10<sup>9</sup></code>
*   <code>1 <= meetings.length <= 10<sup>5</sup></code>
*   `meetings[i].length == 2`
*   `1 <= meetings[i][0] <= meetings[i][1] <= days`

## Solution

```kotlin
class Solution {
    fun countDays(days: Int, meetings: Array<IntArray>): Int {
        var availableDays: MutableList<IntArray> = ArrayList()
        availableDays.add(intArrayOf(1, days))
        // Iterate through each meeting
        for (meeting in meetings) {
            val start = meeting[0]
            val end = meeting[1]
            val newAvailableDays: MutableList<IntArray> = ArrayList()
            // Iterate through available days and split the intervals
            for (interval in availableDays) {
                if (start > interval[1] || end < interval[0]) {
                    // No overlap, keep the interval
                    newAvailableDays.add(interval)
                } else {
                    // Overlap, split the interval
                    if (interval[0] < start) {
                        newAvailableDays.add(intArrayOf(interval[0], start - 1))
                    }
                    if (interval[1] > end) {
                        newAvailableDays.add(intArrayOf(end + 1, interval[1]))
                    }
                }
            }
            availableDays = newAvailableDays
        }
        // Count the remaining available days
        var availableDaysCount = 0
        for (interval in availableDays) {
            availableDaysCount += interval[1] - interval[0] + 1
        }
        return availableDaysCount
    }
}
```