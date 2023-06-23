[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1870\. Minimum Speed to Arrive on Time

Medium

You are given a floating-point number `hour`, representing the amount of time you have to reach the office. To commute to the office, you must take `n` trains in sequential order. You are also given an integer array `dist` of length `n`, where `dist[i]` describes the distance (in kilometers) of the <code>i<sup>th</sup></code> train ride.

Each train can only depart at an integer hour, so you may need to wait in between each train ride.

*   For example, if the <code>1<sup>st</sup></code> train ride takes `1.5` hours, you must wait for an additional `0.5` hours before you can depart on the <code>2<sup>nd</sup></code> train ride at the 2 hour mark.

Return _the **minimum positive integer** speed **(in kilometers per hour)** that all the trains must travel at for you to reach the office on time, or_ `-1` _if it is impossible to be on time_.

Tests are generated such that the answer will not exceed <code>10<sup>7</sup></code> and `hour` will have **at most two digits after the decimal point**.

**Example 1:**

**Input:** dist = [1,3,2], hour = 6

**Output:** 1

**Explanation:** At speed 1:

- The first train ride takes 1/1 = 1 hour.

- Since we are already at an integer hour, we depart immediately at the 1 hour mark. The second train takes 3/1 = 3 hours.

- Since we are already at an integer hour, we depart immediately at the 4 hour mark. The third train takes 2/1 = 2 hours.

- You will arrive at exactly the 6 hour mark. 

**Example 2:**

**Input:** dist = [1,3,2], hour = 2.7

**Output:** 3

**Explanation:** At speed 3:

- The first train ride takes 1/3 = 0.33333 hours.

- Since we are not at an integer hour, we wait until the 1 hour mark to depart. The second train ride takes 3/3 = 1 hour.

- Since we are already at an integer hour, we depart immediately at the 2 hour mark. The third train takes 2/3 = 0.66667 hours.

- You will arrive at the 2.66667 hour mark. 

**Example 3:**

**Input:** dist = [1,3,2], hour = 1.9

**Output:** -1

**Explanation:** It is impossible because the earliest the third train can depart is at the 2 hour mark. 

**Constraints:**

*   `n == dist.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= dist[i] <= 10<sup>5</sup></code>
*   <code>1 <= hour <= 10<sup>9</sup></code>
*   There will be at most two digits after the decimal point in `hour`.

## Solution

```kotlin
class Solution {
    fun minSpeedOnTime(dist: IntArray, hour: Double): Int {
        val n = dist.size
        return fmin(dist, n, hour)
    }

    private fun check(dist: IntArray, n: Int, h: Double, spe: Int): Boolean {
        var cost = 0.0
        for (i in 0 until n - 1) {
            // same as ceil(doubleTime/doubleSpeed)
            cost += ((dist[i] - 1) / spe + 1).toDouble()
        }
        cost += dist[n - 1].toDouble() / spe.toDouble()
        return cost <= h
    }

    private fun fmin(dist: IntArray, n: Int, h: Double): Int {
        if (h + 1 <= n) {
            return -1
        }
        val max = fmax(dist) * 100
        var lo = 1
        var hi = max
        while (lo < hi) {
            val mid = (lo + hi) / 2
            // speed of mid is possible, move to left side
            if (check(dist, n, h, mid)) {
                hi = mid
            } else {
                // need higher speed, move to right side
                lo = mid + 1
            }
        }
        return lo
    }

    private fun fmax(arr: IntArray): Int {
        var res = arr[0]
        for (num in arr) {
            res = Math.max(res, num)
        }
        return res
    }
}
```