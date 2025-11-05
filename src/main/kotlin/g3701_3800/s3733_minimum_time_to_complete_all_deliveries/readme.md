[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3733\. Minimum Time to Complete All Deliveries

Medium

You are given two integer arrays of size 2: <code>d = [d<sub>1</sub>, d<sub>2</sub>]</code> and <code>r = [r<sub>1</sub>, r<sub>2</sub>]</code>.

Two delivery drones are tasked with completing a specific number of deliveries. Drone `i` must complete <code>d<sub>i</sub></code> deliveries.

Each delivery takes **exactly** one hour and **only one** drone can make a delivery at any given hour.

Additionally, both drones require recharging at specific intervals during which they cannot make deliveries. Drone `i` must recharge every <code>r<sub>i</sub></code> hours (i.e. at hours that are multiples of <code>r<sub>i</sub></code>).

Return an integer denoting the **minimum** total time (in hours) required to complete all deliveries.

**Example 1:**

**Input:** d = [3,1], r = [2,3]

**Output:** 5

**Explanation:**

*   The first drone delivers at hours 1, 3, 5 (recharges at hours 2, 4).
*   The second drone delivers at hour 2 (recharges at hour 3).

**Example 2:**

**Input:** d = [1,3], r = [2,2]

**Output:** 7

**Explanation:**

*   The first drone delivers at hour 3 (recharges at hours 2, 4, 6).
*   The second drone delivers at hours 1, 5, 7 (recharges at hours 2, 4, 6).

**Example 3:**

**Input:** d = [2,1], r = [3,4]

**Output:** 3

**Explanation:**

*   The first drone delivers at hours 1, 2 (recharges at hour 3).
*   The second drone delivers at hour 3.

**Constraints:**

*   <code>d = [d<sub>1</sub>, d<sub>2</sub>]</code>
*   <code>1 <= d<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>r = [r<sub>1</sub>, r<sub>2</sub>]</code>
*   <code>2 <= r<sub>i</sub> <= 3 * 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private fun pos(k: Long, n1: Long, n2: Long, p1: Int, p2: Int, lVal: Long): Boolean {
        val kP1 = k / p1
        val kP2 = k / p2
        val kL = k / lVal
        val s1 = kP2 - kL
        val s2 = kP1 - kL
        val sB = k - kP1 - kP2 + kL
        val w1 = max(0L, n1 - s1)
        val w2 = max(0L, n2 - s2)
        return (w1 + w2) <= sB
    }

    private fun findGcd(a: Long, b: Long): Long {
        if (b == 0L) {
            return a
        }
        return findGcd(b, a % b)
    }

    fun minimumTime(d: IntArray, r: IntArray): Long {
        val n1 = d[0].toLong()
        val n2 = d[1].toLong()
        val p1 = r[0]
        val p2 = r[1]
        val g = findGcd(p1.toLong(), p2.toLong())
        val l = p1.toLong() * p2 / g
        var lo = n1 + n2
        var hi = (n1 + n2) * max(p1, p2)
        var res = hi
        while (lo <= hi) {
            val mid = lo + (hi - lo) / 2
            if (pos(mid, n1, n2, p1, p2, l)) {
                res = mid
                hi = mid - 1
            } else {
                lo = mid + 1
            }
        }
        return res
    }
}
```