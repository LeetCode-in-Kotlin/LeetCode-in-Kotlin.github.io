[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4027\. Elevator Requests III

Hard

You are given an integer `n` denoting the number of floors in a building, where the floors are numbered from 0 to `n - 1`.

You are also given an integer `start` and a 2D integer array `requests`, where <code>requests[i] = [arrival<sub>i</sub>, floor<sub>i</sub>]</code> indicates that a request for <code>floor<sub>i</sub></code> is made at time <code>arrival<sub>i</sub></code>.

At time 0, the elevator is at floor `start`.

At each second, the elevator may move **up** by 1 floor, move **down** by 1 floor, or **remain** on its current floor.

A request can be fulfilled **only** at or after its arrival time; it is fulfilled **instantly** when the elevator is on its requested floor at any time from its arrival time onward.

Return the **minimum** time needed to fulfill all requests.

**Example 1:**

**Input:** n = 9, start = 0, requests = \[\[0,8],[6,5]]

**Output:** 9

**Explanation:**

*   Move from floor 0 (`start`) to floor 5 (`requests[1][1]`) in 5 seconds, reaching at time 5. Since `requests[1][0] = 6`, wait until time 6 to fulfill it.
*   Move from floor 5 to floor 8 (`requests[0][1]`) in 3 seconds, fulfilling it at time 9.

Thus, all requests are fulfilled by time 9.

**Example 2:**

**Input:** n = 8, start = 5, requests = \[\[1,7],[7,3]]

**Output:** 7

**Explanation:**

*   Move from floor 5 (`start`) to floor 7 (`requests[0][1]`) in 2 seconds, reaching at time 2. Since `requests[0][0] = 1` has already passed, floor 7 is fulfilled at time 2.
*   Move from floor 7 to floor 3 (`requests[1][1]`) in 4 seconds, reaching at time 6. Since `requests[1][0] = 7`, wait until time 7.

Thus, all requests are fulfilled by time 7.

**Example 3:**

**Input:** n = 7, start = 3, requests = \[\[0,5],[0,1],[6,3]]

**Output:** 8

**Explanation:**

*   Move from floor 3 (`start`) to floor 5 (`requests[0][1]`) in 2 seconds, fulfilling it at time 2.
*   Move from floor 5 to floor 1 (`requests[1][1]`) in 4 seconds, fulfilling it at time 6.
*   Move from floor 1 to floor 3 (`requests[2][1]`) in 2 seconds, reaching at time 8. Its request arrived at `requests[2][0] = 6`, so floor 3 is fulfilled at time 8.

Thus, all requests are fulfilled by time 8.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   `1 <= requests.length <= 16`
*   <code>requests[i] == [arrival<sub>i</sub>, floor<sub>i</sub>]</code>
*   <code>0 <= arrival<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>0 <= start, floor<sub>i</sub> <= n - 1</code>

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun elevatorRequests(n: Int, start: Int, requests: Array<IntArray>): Long = solve(start, 0L, 0, requests)

    private fun solve(currFloor: Int, currTime: Long, fulfilledMask: Int, requests: Array<IntArray>): Long {
        val m = requests.size
        if (fulfilledMask == (1 shl m) - 1) {
            return currTime
        }
        var minFloor = Int.MAX_VALUE
        var maxFloor = Int.MIN_VALUE
        for (i in 0..<m) {
            if ((fulfilledMask and (1 shl i)) == 0) {
                minFloor = min(minFloor, requests[i][1])
                maxFloor = max(maxFloor, requests[i][1])
            }
        }
        var res = Long.MAX_VALUE
        val targets =
            if (minFloor == maxFloor) intArrayOf(minFloor) else intArrayOf(minFloor, maxFloor)
        for (target in targets) {
            var nextMask = fulfilledMask
            var waitTargetTime = currTime + abs(target - currFloor)
            for (i in 0..<m) {
                if ((fulfilledMask and (1 shl i)) == 0) {
                    val f = requests[i][1]
                    val arr = requests[i][0].toLong()
                    if ((currFloor <= f && f <= target) || (target <= f && f <= currFloor)) {
                        val passTime = currTime + abs(f - currFloor)
                        if (passTime >= arr) {
                            nextMask = nextMask or (1 shl i)
                        } else if (f == target) {
                            waitTargetTime = max(waitTargetTime, arr)
                            nextMask = nextMask or (1 shl i)
                        }
                    }
                }
            }
            res = min(res, solve(target, waitTargetTime, nextMask, requests))
        }
        return res
    }
}
```