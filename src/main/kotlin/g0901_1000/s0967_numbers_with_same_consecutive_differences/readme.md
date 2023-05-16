[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 967\. Numbers With Same Consecutive Differences

Medium

Given two integers n and k, return _an array of all the integers of length_ `n` _where the difference between every two consecutive digits is_ `k`. You may return the answer in **any order**.

Note that the integers should not have leading zeros. Integers as `02` and `043` are not allowed.

**Example 1:**

**Input:** n = 3, k = 7

**Output:** [181,292,707,818,929]

**Explanation:** Note that 070 is not a valid number, because it has leading zeroes.

**Example 2:**

**Input:** n = 2, k = 1

**Output:** [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

**Constraints:**

*   `2 <= n <= 9`
*   `0 <= k <= 9`

## Solution

```kotlin
import kotlin.math.abs

@Suppress("NAME_SHADOWING")
class Solution {
    fun numsSameConsecDiff(n: Int, k: Int): IntArray {
        var k = k
        k = abs(k)
        val list: MutableList<Int> = ArrayList()
        dfs(list, 100000, 0, n, k)
        val res = IntArray(list.size)
        for (i in res.indices) {
            res[i] = list[i]
        }
        return res
    }

    private fun dfs(list: MutableList<Int>, can: Int, len: Int, n: Int, k: Int) {
        if (len == n) {
            list.add(can)
            return
        }
        if (can == 0) {
            return
        }
        if (len == 0) {
            for (i in 0..9) {
                dfs(list, i, 1, n, k)
            }
        } else {
            val last = can % 10
            val a = last + k
            val b = last - k
            if (b >= 0) {
                dfs(list, can * 10 + b, len + 1, n, k)
            }
            if (k != 0 && a < 10) {
                dfs(list, can * 10 + a, len + 1, n, k)
            }
        }
    }
}
```