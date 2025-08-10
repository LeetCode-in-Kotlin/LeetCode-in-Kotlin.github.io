[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3633\. Earliest Finish Time for Land and Water Rides I

Easy

You are given two categories of theme park attractions: **land rides** and **water rides**.

*   **Land rides**
    *   `landStartTime[i]` – the earliest time the <code>i<sup>th</sup></code> land ride can be boarded.
    *   `landDuration[i]` – how long the <code>i<sup>th</sup></code> land ride lasts.
*   **Water rides**
    *   `waterStartTime[j]` – the earliest time the <code>j<sup>th</sup></code> water ride can be boarded.
    *   `waterDuration[j]` – how long the <code>j<sup>th</sup></code> water ride lasts.

A tourist must experience **exactly one** ride from **each** category, in **either order**.

*   A ride may be started at its opening time or **any later moment**.
*   If a ride is started at time `t`, it finishes at time `t + duration`.
*   Immediately after finishing one ride the tourist may board the other (if it is already open) or wait until it opens.

Return the **earliest possible time** at which the tourist can finish both rides.

**Example 1:**

**Input:** landStartTime = [2,8], landDuration = [4,1], waterStartTime = [6], waterDuration = [3]

**Output:** 9

**Explanation:**

*   Plan A (land ride 0 → water ride 0):
    *   Start land ride 0 at time `landStartTime[0] = 2`. Finish at `2 + landDuration[0] = 6`.
    *   Water ride 0 opens at time `waterStartTime[0] = 6`. Start immediately at `6`, finish at `6 + waterDuration[0] = 9`.
*   Plan B (water ride 0 → land ride 1):
    *   Start water ride 0 at time `waterStartTime[0] = 6`. Finish at `6 + waterDuration[0] = 9`.
    *   Land ride 1 opens at `landStartTime[1] = 8`. Start at time `9`, finish at `9 + landDuration[1] = 10`.
*   Plan C (land ride 1 → water ride 0):
    *   Start land ride 1 at time `landStartTime[1] = 8`. Finish at `8 + landDuration[1] = 9`.
    *   Water ride 0 opened at `waterStartTime[0] = 6`. Start at time `9`, finish at `9 + waterDuration[0] = 12`.
*   Plan D (water ride 0 → land ride 0):
    *   Start water ride 0 at time `waterStartTime[0] = 6`. Finish at `6 + waterDuration[0] = 9`.
    *   Land ride 0 opened at `landStartTime[0] = 2`. Start at time `9`, finish at `9 + landDuration[0] = 13`.

Plan A gives the earliest finish time of 9.

**Example 2:**

**Input:** landStartTime = [5], landDuration = [3], waterStartTime = [1], waterDuration = [10]

**Output:** 14

**Explanation:**

*   Plan A (water ride 0 → land ride 0):
    *   Start water ride 0 at time `waterStartTime[0] = 1`. Finish at `1 + waterDuration[0] = 11`.
    *   Land ride 0 opened at `landStartTime[0] = 5`. Start immediately at `11` and finish at `11 + landDuration[0] = 14`.
*   Plan B (land ride 0 → water ride 0):
    *   Start land ride 0 at time `landStartTime[0] = 5`. Finish at `5 + landDuration[0] = 8`.
    *   Water ride 0 opened at `waterStartTime[0] = 1`. Start immediately at `8` and finish at `8 + waterDuration[0] = 18`.

Plan A provides the earliest finish time of 14.

**Constraints:**

*   `1 <= n, m <= 100`
*   `landStartTime.length == landDuration.length == n`
*   `waterStartTime.length == waterDuration.length == m`
*   `1 <= landStartTime[i], landDuration[i], waterStartTime[j], waterDuration[j] <= 1000`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun earliestFinishTime(
        landStartTime: IntArray,
        landDuration: IntArray,
        waterStartTime: IntArray,
        waterDuration: IntArray,
    ): Int {
        var res = Int.Companion.MAX_VALUE
        val n = landStartTime.size
        val m = waterStartTime.size
        // Try all combinations of one land and one water ride
        for (i in 0..<n) {
            // start time of land ride
            val a = landStartTime[i]
            // duration of land ride
            val d = landDuration[i]
            for (j in 0..<m) {
                // start time of water ride
                val b = waterStartTime[j]
                // duration of water ride
                val e = waterDuration[j]
                // Case 1: Land → Water
                val landEnd = a + d
                // wait if needed
                val startWater = max(landEnd, b)
                val finish1 = startWater + e
                // Case 2: Water → Land
                val waterEnd = b + e
                // wait if needed
                val startLand = max(waterEnd, a)
                val finish2 = startLand + d
                // Take the minimum finish time
                res = min(res, min(finish1, finish2))
            }
        }
        return res
    }
}
```