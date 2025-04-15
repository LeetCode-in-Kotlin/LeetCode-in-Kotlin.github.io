[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3413\. Maximum Coins From K Consecutive Bags

Medium

There are an infinite amount of bags on a number line, one bag for each coordinate. Some of these bags contain coins.

You are given a 2D array `coins`, where <code>coins[i] = [l<sub>i</sub>, r<sub>i</sub>, c<sub>i</sub>]</code> denotes that every bag from <code>l<sub>i</sub></code> to <code>r<sub>i</sub></code> contains <code>c<sub>i</sub></code> coins.

The segments that `coins` contain are non-overlapping.

You are also given an integer `k`.

Return the **maximum** amount of coins you can obtain by collecting `k` consecutive bags.

**Example 1:**

**Input:** coins = \[\[8,10,1],[1,3,2],[5,6,4]], k = 4

**Output:** 10

**Explanation:**

Selecting bags at positions `[3, 4, 5, 6]` gives the maximum number of coins: `2 + 0 + 4 + 4 = 10`.

**Example 2:**

**Input:** coins = \[\[1,10,3]], k = 2

**Output:** 6

**Explanation:**

Selecting bags at positions `[1, 2]` gives the maximum number of coins: `3 + 3 = 6`.

**Constraints:**

*   <code>1 <= coins.length <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>
*   <code>coins[i] == [l<sub>i</sub>, r<sub>i</sub>, c<sub>i</sub>]</code>
*   <code>1 <= l<sub>i</sub> <= r<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= c<sub>i</sub> <= 1000</code>
*   The given segments are non-overlapping.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumCoins(coins: Array<IntArray>, k: Int): Long {
        coins.sortWith { a: IntArray, b: IntArray -> a[0] - b[0] }
        val n = coins.size
        var res: Long = 0
        var cur: Long = 0
        var j = 0
        for (ints in coins) {
            while (j < n && coins[j][1] <= ints[0] + k - 1) {
                cur += (coins[j][1] - coins[j][0] + 1) * coins[j][2]
                j++
            }
            if (j < n) {
                val part = max(0, ints[0] + k - 1 - coins[j][0] + 1) * coins[j][2]
                res = max(res, cur + part)
            }
            cur -= (ints[1] - ints[0] + 1) * ints[2]
        }
        cur = 0
        j = 0
        for (coin in coins) {
            cur += (coin[1] - coin[0] + 1) * coin[2]
            while (coins[j][1] < coin[1] - k + 1) {
                cur -= (coins[j][1] - coins[j][0] + 1) * coins[j][2]
                j++
            }
            val part = max(0, coin[1] - k - coins[j][0] + 1) * coins[j][2]
            res = max(res, (cur - part))
        }
        return res
    }
}
```