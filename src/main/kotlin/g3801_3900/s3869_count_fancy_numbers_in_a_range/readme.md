[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3869\. Count Fancy Numbers in a Range

Hard

You are given two integers `l` and `r`.

An integer is called **good** if its digits form a **strictly monotone** sequence, meaning the digits are **strictly increasing** or **strictly decreasing**. All single-digit integers are considered good.

An integer is called **fancy** if it is good, or if the **sum of its digits** is good.

Return an integer representing the number of fancy integers in the range `[l, r]` (inclusive).

A sequence is said to be **strictly increasing** if each element is **strictly greater** than its previous one (if exists).

A sequence is said to be **strictly decreasing** if each element is **strictly less** than its previous one (if exists).

**Example 1:**

**Input:** l = 8, r = 10

**Output:** 3

**Explanation:**

*   8 and 9 are single-digit integers, so they are good and therefore fancy.
*   10 has digits `[1, 0]`, which form a strictly decreasing sequence, so 10 is good and thus fancy.

Therefore, the answer is 3.

**Example 2:**

**Input:** l = 12340, r = 12341

**Output:** 1

**Explanation:**

*   12340
    *   12340 is not good because `[1, 2, 3, 4, 0]` is not strictly monotone.
    *   The digit sum is `1 + 2 + 3 + 4 + 0 = 10`.
    *   10 is good as it has digits `[1, 0]`, which is strictly decreasing. Therefore, 12340 is fancy.
*   12341
    *   12341 is not good because `[1, 2, 3, 4, 1]` is not strictly monotone.
    *   The digit sum is `1 + 2 + 3 + 4 + 1 = 11`.
    *   11 is not good as it has digits `[1, 1]`, which is not strictly monotone. Therefore, 12341 is not fancy.

Therefore, the answer is 1.

**Example 3:**

**Input:** l = 123456788, r = 123456788

**Output:** 0

**Explanation:**

*   123456788 is not good because its digits are not strictly monotone.
*   The digit sum is `1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 8 = 44`.
*   44 is not good as it has digits `[4, 4]`, which is not strictly monotone. Therefore, 123456788 is not fancy.

Therefore, the answer is 0.

**Constraints:**

*   <code>1 <= l <= r <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var low: CharArray
    private lateinit var high: CharArray
    private lateinit var dp: Array<Array<Array<LongArray>>>
    private lateinit var good: BooleanArray

    fun countFancy(l: Long, r: Long): Long {
        initBounds(l, r)
        initGood()
        initDp()
        val ans = dfs(0, 0, 1, 1)
        val seen: MutableSet<Long> = HashSet<Long>()
        addIncreasingNumbers(seen, l, r)
        addDecreasingNumbers(seen, l, r)
        return ans + seen.size
    }

    private fun initBounds(l: Long, r: Long) {
        var left = l.toString()
        val right = r.toString()
        val n = right.length
        left = "0".repeat(n - left.length) + left
        low = left.toCharArray()
        high = right.toCharArray()
    }

    private fun initGood() {
        good = BooleanArray(136)
        for (i in 1..135) {
            good[i] = isGood(i)
        }
    }

    private fun initDp() {
        dp = Array<Array<Array<LongArray>>>(high.size + 1) {
            Array<Array<LongArray>>(136) {
                Array<LongArray>(2) {
                    LongArray(2)
                }
            }
        }
        for (a in dp) {
            for (b in a) {
                for (c in b) {
                    c.fill(-1)
                }
            }
        }
    }

    private fun addIncreasingNumbers(seen: MutableSet<Long>, l: Long, r: Long) {
        for (mask in 1..<(1 shl 9)) {
            var x: Long = 0
            var sum = 0
            for (i in 0..8) {
                if ((mask and (1 shl i)) != 0) {
                    x = x * 10 + i + 1
                    sum += i + 1
                }
            }
            if (x >= l && x <= r && !good[sum]) {
                seen.add(x)
            }
        }
    }

    private fun addDecreasingNumbers(seen: MutableSet<Long>, l: Long, r: Long) {
        for (mask in 1..<(1 shl 10)) {
            var x: Long = 0
            var sum = 0
            var started = false
            for (i in 0..9) {
                if ((mask and (1 shl i)) != 0) {
                    val d = 9 - i
                    if (started || d != 0) {
                        started = true
                        x = x * 10 + d
                        sum += d
                    }
                }
            }
            if (started && x >= l && x <= r && !good[sum]) {
                seen.add(x)
            }
        }
    }

    private fun dfs(pos: Int, sum: Int, tl: Int, tr: Int): Long {
        if (pos == high.size) {
            return (if (good[sum]) 1 else 0).toLong()
        }
        if (dp[pos][sum][tl][tr] != -1L) {
            return dp[pos][sum][tl][tr]
        }
        val lo = if (tl == 1) low[pos].code - '0'.code else 0
        val hi = if (tr == 1) high[pos].code - '0'.code else 9
        var res: Long = 0
        for (d in lo..hi) {
            res += dfs(pos + 1, sum + d, nextTight(tl, d, lo), nextTight(tr, d, hi))
        }
        dp[pos][sum][tl][tr] = res
        return res
    }

    private fun nextTight(tight: Int, digit: Int, limit: Int): Int = if (tight == 1 && digit == limit) 1 else 0

    private fun isGood(x: Int): Boolean {
        if (x < 10) {
            return true
        }
        val s = x.toString().toCharArray()
        var inc = true
        var dec = true
        for (i in 1..<s.size) {
            if (s[i] <= s[i - 1]) {
                inc = false
            }
            if (s[i] >= s[i - 1]) {
                dec = false
            }
        }
        return inc || dec
    }
}
```