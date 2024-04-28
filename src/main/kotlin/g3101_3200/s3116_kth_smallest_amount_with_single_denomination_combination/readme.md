[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3116\. Kth Smallest Amount With Single Denomination Combination

Hard

You are given an integer array `coins` representing coins of different denominations and an integer `k`.

You have an infinite number of coins of each denomination. However, you are **not allowed** to combine coins of different denominations.

Return the <code>k<sup>th</sup></code> **smallest** amount that can be made using these coins.

**Example 1:**

**Input:** coins = [3,6,9], k = 3

**Output:** 9

**Explanation:** The given coins can make the following amounts:   

 Coin 3 produces multiples of 3: 3, 6, 9, 12, 15, etc.   

 Coin 6 produces multiples of 6: 6, 12, 18, 24, etc.   

 Coin 9 produces multiples of 9: 9, 18, 27, 36, etc.   

 All of the coins combined produce: 3, 6, <ins>**9**</ins>, 12, 15, etc.

**Example 2:**

**Input:** coins = [5,2], k = 7

**Output:** 12

**Explanation:** The given coins can make the following amounts:   

 Coin 5 produces multiples of 5: 5, 10, 15, 20, etc.   

 Coin 2 produces multiples of 2: 2, 4, 6, 8, 10, 12, etc.   

 All of the coins combined produce: 2, 4, 5, 6, 8, 10, <ins>**12**</ins>, 14, 15, etc.

**Constraints:**

*   `1 <= coins.length <= 15`
*   `1 <= coins[i] <= 25`
*   <code>1 <= k <= 2 * 10<sup>9</sup></code>
*   `coins` contains pairwise distinct integers.

## Solution

```kotlin
import kotlin.math.min

@Suppress("NAME_SHADOWING")
class Solution {
    fun findKthSmallest(coins: IntArray, k: Int): Long {
        var minC = Int.MAX_VALUE
        for (c in coins) {
            minC = min(minC, c)
        }
        val cc = coins(coins)
        var max = minC.toLong() * k
        var min = max / coins.size
        while (min < max) {
            val mid = (min + max) / 2
            val cnt = count(cc, mid)
            if (cnt > k) {
                max = mid - 1
            } else if (cnt < k) {
                min = mid + 1
            } else {
                max = mid
            }
        }
        return min
    }

    private fun count(coins: LongArray, v: Long): Long {
        var r: Long = 0
        for (c in coins) {
            r += v / c
        }
        return r
    }

    private fun coins(coins: IntArray): LongArray {
        var coins = coins
        coins.sort()
        var len = 1
        a@ for (i in 1 until coins.size) {
            val c = coins[i]
            for (j in 0 until len) {
                if (c % coins[j] == 0) {
                    continue@a
                }
            }
            coins[len++] = c
        }
        coins = coins.copyOf(len)
        val res = LongArray((1 shl coins.size) - 1)
        iterate(coins, res, 1, 0, 0, true)
        return res
    }

    private fun iterate(coins: IntArray, res: LongArray, mult: Long, start: Int, idx: Int, positive: Boolean): Int {
        var idx = idx
        for (i in start until coins.size) {
            val next = mult * coins[i] / gcd(mult, coins[i].toLong())
            res[idx++] = if (positive) next else -next
            idx = iterate(coins, res, next, i + 1, idx, !positive)
        }
        return idx
    }

    private fun gcd(a: Long, b: Long): Long {
        return if (b == 0L) a else gcd(b, a % b)
    }
}
```