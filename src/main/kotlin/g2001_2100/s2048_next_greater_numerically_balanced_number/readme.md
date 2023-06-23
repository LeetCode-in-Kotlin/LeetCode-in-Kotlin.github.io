[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2048\. Next Greater Numerically Balanced Number

Medium

An integer `x` is **numerically balanced** if for every digit `d` in the number `x`, there are **exactly** `d` occurrences of that digit in `x`.

Given an integer `n`, return _the **smallest numerically balanced** number **strictly greater** than_ `n`_._

**Example 1:**

**Input:** n = 1

**Output:** 22

**Explanation:** 

22 is numerically balanced since: 

- The digit 2 occurs 2 times. 
  
It is also the smallest numerically balanced number strictly greater than 1.

**Example 2:**

**Input:** n = 1000

**Output:** 1333

**Explanation:** 

1333 is numerically balanced since:

- The digit 1 occurs 1 time. 

- The digit 3 occurs 3 times. 
  
It is also the smallest numerically balanced number strictly greater than 1000. Note that 1022 cannot be the answer because 0 appeared more than 0 times.

**Example 3:**

**Input:** n = 3000

**Output:** 3133

**Explanation:** 

3133 is numerically balanced since: 

- The digit 1 occurs 1 time. 

- The digit 3 occurs 3 times. 
  
It is also the smallest numerically balanced number strictly greater than 3000.

**Constraints:**

*   <code>0 <= n <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun nextBeautifulNumber(n: Int): Int {
        val arr = intArrayOf(0, 1, 2, 3, 4, 5, 6)
        val select = BooleanArray(7)
        val d = if (n == 0) 1 else Math.log10(n.toDouble()).toInt() + 1
        return solve(1, n, d, 0, select, arr)
    }

    private fun solve(i: Int, n: Int, d: Int, sz: Int, select: BooleanArray, arr: IntArray): Int {
        if (sz > d + 1) {
            return Int.MAX_VALUE
        }
        if (i == select.size) {
            return if (sz >= d) make(0, n, sz, select, arr) else Int.MAX_VALUE
        }
        var ans = solve(i + 1, n, d, sz, select, arr)
        select[i] = true
        ans = Math.min(ans, solve(i + 1, n, d, sz + i, select, arr))
        select[i] = false
        return ans
    }

    private fun make(cur: Int, n: Int, end: Int, select: BooleanArray, arr: IntArray): Int {
        if (end == 0) {
            return if (cur > n) cur else Int.MAX_VALUE
        }
        var ans = Int.MAX_VALUE
        for (j in 1 until arr.size) {
            if (!select[j] || arr[j] == 0) {
                continue
            }
            --arr[j]
            ans = Math.min(make(10 * cur + j, n, end - 1, select, arr), ans)
            ++arr[j]
        }
        return ans
    }
}
```