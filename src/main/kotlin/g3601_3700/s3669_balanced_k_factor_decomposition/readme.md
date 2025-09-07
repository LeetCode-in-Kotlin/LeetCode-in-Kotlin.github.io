[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3669\. Balanced K-Factor Decomposition

Medium

Given two integers `n` and `k`, split the number `n` into exactly `k` positive integers such that the **product** of these integers is equal to `n`.

Return _any_ _one_ split in which the **maximum** difference between any two numbers is **minimized**. You may return the result in _any order_.

**Example 1:**

**Input:** n = 100, k = 2

**Output:** [10,10]

**Explanation:**

The split `[10, 10]` yields `10 * 10 = 100` and a max-min difference of 0, which is minimal.

**Example 2:**

**Input:** n = 44, k = 3

**Output:** [2,2,11]

**Explanation:**

*   Split `[1, 1, 44]` yields a difference of 43
*   Split `[1, 2, 22]` yields a difference of 21
*   Split `[1, 4, 11]` yields a difference of 10
*   Split `[2, 2, 11]` yields a difference of 9

Therefore, `[2, 2, 11]` is the optimal split with the smallest difference 9.

**Constraints:**

*   <code>4 <= n <= 10<sup>5</sup></code>
*   `2 <= k <= 5`
*   `k` is strictly less than the total number of positive divisors of `n`.

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    private var kGlobal = 0
    private var bestDiff = Int.Companion.MAX_VALUE
    private var bestList: MutableList<Int> = ArrayList<Int>()
    private val current: MutableList<Int> = ArrayList<Int>()

    fun minDifference(n: Int, k: Int): IntArray {
        kGlobal = k
        dfs(n, 1, 0)
        val ans = IntArray(bestList.size)
        for (i in bestList.indices) {
            ans[i] = bestList[i]
        }
        return ans
    }

    private fun dfs(rem: Int, start: Int, depth: Int) {
        if (depth == kGlobal - 1) {
            if (rem >= start) {
                current.add(rem)
                evaluate()
                current.removeAt(current.size - 1)
            }
            return
        }
        val divs = getDivisors(rem)
        for (d in divs) {
            if (d < start) {
                continue
            }
            current.add(d)
            dfs(rem / d, d, depth + 1)
            current.removeAt(current.size - 1)
        }
    }

    private fun evaluate() {
        var mn = Int.Companion.MAX_VALUE
        var mx = Int.Companion.MIN_VALUE
        for (v in current) {
            mn = min(mn, v)
            mx = max(mx, v)
        }
        val diff = mx - mn
        if (diff < bestDiff) {
            bestDiff = diff
            bestList = ArrayList<Int>(current)
        }
    }

    private fun getDivisors(x: Int): MutableList<Int> {
        val small: MutableList<Int> = ArrayList<Int>()
        val large: MutableList<Int> = ArrayList<Int>()
        var i = 1
        while (i * i.toLong() <= x) {
            if (x % i == 0) {
                small.add(i)
                if (i != x / i) {
                    large.add(x / i)
                }
            }
            i++
        }
        large.reverse()
        small.addAll(large)
        return small
    }
}
```