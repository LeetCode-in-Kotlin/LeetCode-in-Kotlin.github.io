[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2054\. Two Best Non-Overlapping Events

Medium

You are given a **0-indexed** 2D integer array of `events` where <code>events[i] = [startTime<sub>i</sub>, endTime<sub>i</sub>, value<sub>i</sub>]</code>. The <code>i<sup>th</sup></code> event starts at <code>startTime<sub>i</sub></code> and ends at <code>endTime<sub>i</sub></code>, and if you attend this event, you will receive a value of <code>value<sub>i</sub></code>. You can choose **at most** **two** **non-overlapping** events to attend such that the sum of their values is **maximized**.

Return _this **maximum** sum._

Note that the start time and end time is **inclusive**: that is, you cannot attend two events where one of them starts and the other ends at the same time. More specifically, if you attend an event with end time `t`, the next event must start at or after `t + 1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/21/picture5.png)

**Input:** events = \[\[1,3,2],[4,5,2],[2,4,3]]

**Output:** 4

**Explanation:** Choose the green events, 0 and 1 for a sum of 2 + 2 = 4. 

**Example 2:**

![Example 1 Diagram](https://assets.leetcode.com/uploads/2021/09/21/picture1.png)

**Input:** events = \[\[1,3,2],[4,5,2],[1,5,5]]

**Output:** 5

**Explanation:** Choose event 2 for a sum of 5. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/21/picture3.png)

**Input:** events = \[\[1,5,3],[1,5,1],[6,6,5]]

**Output:** 8

**Explanation:** Choose events 0 and 2 for a sum of 3 + 5 = 8.

**Constraints:**

*   <code>2 <= events.length <= 10<sup>5</sup></code>
*   `events[i].length == 3`
*   <code>1 <= startTime<sub>i</sub> <= endTime<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= value<sub>i</sub> <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun maxTwoEvents(events: Array<IntArray>): Int {
        events.sortWith { a: IntArray, b: IntArray -> a[0] - b[0] }
        val max = IntArray(events.size)
        for (i in events.indices.reversed()) {
            if (i == events.size - 1) {
                max[i] = events[i][2]
            } else {
                max[i] = events[i][2].coerceAtLeast(max[i + 1])
            }
        }
        var ans = 0
        for (i in events.indices) {
            val end = events[i][1]
            var left = i + 1
            var right = events.size
            while (left < right) {
                val mid = left + (right - left) / 2
                if (events[mid][0] <= end) {
                    left = mid + 1
                } else {
                    right = mid
                }
            }
            var value = events[i][2]
            if (right < events.size) {
                value += max[right]
            }
            ans = ans.coerceAtLeast(value)
        }
        return ans
    }
}
```