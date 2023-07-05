[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2528\. Maximize the Minimum Powered City

Hard

You are given a **0-indexed** integer array `stations` of length `n`, where `stations[i]` represents the number of power stations in the <code>i<sup>th</sup></code> city.

Each power station can provide power to every city in a fixed **range**. In other words, if the range is denoted by `r`, then a power station at city `i` can provide power to all cities `j` such that `|i - j| <= r` and `0 <= i, j <= n - 1`.

*   Note that `|x|` denotes **absolute** value. For example, `|7 - 5| = 2` and `|3 - 10| = 7`.

The **power** of a city is the total number of power stations it is being provided power from.

The government has sanctioned building `k` more power stations, each of which can be built in any city, and have the same range as the pre-existing ones.

Given the two integers `r` and `k`, return _the **maximum possible minimum power** of a city, if the additional power stations are built optimally._

**Note** that you can build the `k` power stations in multiple cities.

**Example 1:**

**Input:** stations = [1,2,4,5,0], r = 1, k = 2

**Output:** 5

**Explanation:** 

One of the optimal ways is to install both the power stations at city 1. 

So stations will become [1,4,4,5,0]. 

- City 0 is provided by 1 + 4 = 5 power stations. 

- City 1 is provided by 1 + 4 + 4 = 9 power stations. 

- City 2 is provided by 4 + 4 + 5 = 13 power stations. 

- City 3 is provided by 5 + 4 = 9 power stations.

- City 4 is provided by 5 + 0 = 5 power stations. 

So the minimum power of a city is 5. 

Since it is not possible to obtain a larger power, we return 5.

**Example 2:**

**Input:** stations = [4,4,4,4], r = 0, k = 3

**Output:** 4

**Explanation:** It can be proved that we cannot make the minimum power of a city greater than 4.

**Constraints:**

*   `n == stations.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= stations[i] <= 10<sup>5</sup></code>
*   `0 <= r <= n - 1`
*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun canIBeTheMinimum(power: LongArray, minimum: Long, k: Int, r: Int): Boolean {
        var k = k
        val n = power.size
        val extraPower = LongArray(n)
        for (i in 0 until n) {
            if (i > 0) {
                extraPower[i] += extraPower[i - 1]
            }
            val curPower = power[i] + extraPower[i]
            val req = minimum - curPower
            if (req <= 0) {
                continue
            }
            if (req > k) {
                return false
            }
            k -= req.toInt()
            extraPower[i] += req
            if (i + 2 * r + 1 < n) {
                extraPower[i + 2 * r + 1] -= req
            }
        }
        return true
    }

    private fun calculatePowerArray(stations: IntArray, r: Int): LongArray {
        val n = stations.size
        val preSum = LongArray(n)
        for (i in 0 until n) {
            var st = i - r
            val last = i + r + 1
            if (st < 0) {
                st = 0
            }
            preSum[st] += stations[i].toLong()
            if (last < n) {
                preSum[last] -= stations[i].toLong()
            }
        }
        for (i in 1 until n) {
            preSum[i] += preSum[i - 1]
        }
        return preSum
    }

    fun maxPower(stations: IntArray, r: Int, k: Int): Long {
        var min: Long = 0
        var sum = Math.pow(10.0, 10.0).toLong() + Math.pow(10.0, 9.0).toLong()
        val power = calculatePowerArray(stations, r)
        var ans: Long = -1
        while (min <= sum) {
            val mid = min + sum shr 1
            if (canIBeTheMinimum(power, mid, k, r)) {
                ans = mid
                min = mid + 1
            } else {
                sum = mid - 1
            }
        }
        return ans
    }
}
```