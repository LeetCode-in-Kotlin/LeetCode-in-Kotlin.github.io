[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3186\. Maximum Total Damage With Spell Casting

Medium

A magician has various spells.

You are given an array `power`, where each element represents the damage of a spell. Multiple spells can have the same damage value.

It is a known fact that if a magician decides to cast a spell with a damage of `power[i]`, they **cannot** cast any spell with a damage of `power[i] - 2`, `power[i] - 1`, `power[i] + 1`, or `power[i] + 2`.

Each spell can be cast **only once**.

Return the **maximum** possible _total damage_ that a magician can cast.

**Example 1:**

**Input:** power = [1,1,3,4]

**Output:** 6

**Explanation:**

The maximum possible damage of 6 is produced by casting spells 0, 1, 3 with damage 1, 1, 4.

**Example 2:**

**Input:** power = [7,1,6,6]

**Output:** 13

**Explanation:**

The maximum possible damage of 13 is produced by casting spells 1, 2, 3 with damage 1, 6, 6.

**Constraints:**

*   <code>1 <= power.length <= 10<sup>5</sup></code>
*   <code>1 <= power[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maximumTotalDamage(power: IntArray): Long {
        var maxPower = 0
        for (p in power) {
            if (p > maxPower) {
                maxPower = p
            }
        }
        return if ((maxPower <= 1000000)) smallPower(power, maxPower) else bigPower(power)
    }

    private fun smallPower(power: IntArray, maxPower: Int): Long {
        val counts = IntArray(maxPower + 6)
        for (p in power) {
            counts[p]++
        }
        val dp = LongArray(maxPower + 6)
        dp[1] = counts[1].toLong()
        dp[2] = max((counts[2] * 2L).toDouble(), dp[1].toDouble()).toLong()
        for (i in 3..maxPower) {
            dp[i] = max((counts[i] * i + dp[i - 3]).toDouble(), max(dp[i - 1].toDouble(), dp[i - 2].toDouble()))
                .toLong()
        }
        return dp[maxPower]
    }

    private fun bigPower(power: IntArray): Long {
        power.sort()
        val n = power.size
        val prevs = LongArray(4)
        var curPower = power[0]
        var count = 1
        var result: Long = 0
        for (i in 1..n) {
            val p = if ((i == n)) 1000000009 else power[i]
            if (p == curPower) {
                count++
            } else {
                val curVal = max(
                    (curPower.toLong() * count + prevs[3]).toDouble(),
                    max(prevs[1].toDouble(), prevs[2].toDouble())
                )
                    .toLong()
                val diff = min((p - curPower).toDouble(), (prevs.size - 1).toDouble()).toInt()
                val nextCurVal =
                    if ((diff == 1)) 0 else max(prevs[3].toDouble(), max(curVal.toDouble(), prevs[2].toDouble()))
                        .toLong()
                // Shift the values in prevs[].
                var k = prevs.size - 1
                if (diff < prevs.size - 1) {
                    while (k > diff) {
                        prevs[k] = prevs[k-- - diff]
                    }
                    prevs[k--] = curVal
                }
                while (k > 0) {
                    prevs[k--] = nextCurVal
                }
                curPower = p
                count = 1
            }
        }
        for (v in prevs) {
            if (v > result) {
                result = v
            }
        }
        return result
    }
}
```