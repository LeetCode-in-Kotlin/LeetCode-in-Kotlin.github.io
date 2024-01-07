[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2902\. Count of Sub-Multisets With Bounded Sum

Hard

You are given a **0-indexed** array `nums` of non-negative integers, and two integers `l` and `r`.

Return _the **count of sub-multisets** within_ `nums` _where the sum of elements in each subset falls within the inclusive range of_ `[l, r]`.

Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

A **sub-multiset** is an **unordered** collection of elements of the array in which a given value `x` can occur `0, 1, ..., occ[x]` times, where `occ[x]` is the number of occurrences of `x` in the array.

**Note** that:

*   Two **sub-multisets** are the same if sorting both sub-multisets results in identical multisets.
*   The sum of an **empty** multiset is `0`.

**Example 1:**

**Input:** nums = [1,2,2,3], l = 6, r = 6

**Output:** 1

**Explanation:** The only subset of nums that has a sum of 6 is {1, 2, 3}.

**Example 2:**

**Input:** nums = [2,1,4,2,7], l = 1, r = 5

**Output:** 7

**Explanation:** The subsets of nums that have a sum within the range [1, 5] are {1}, {2}, {4}, {2, 2}, {1, 2}, {1, 4}, and {1, 2, 2}.

**Example 3:**

**Input:** nums = [1,2,1,3,5,2], l = 3, r = 5

**Output:** 9

**Explanation:** The subsets of nums that have a sum within the range [3, 5] are {3}, {5}, {1, 2}, {1, 3}, {2, 2}, {2, 3}, {1, 1, 2}, {1, 1, 3}, and {1, 2, 2}.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   Sum of `nums` does not exceed <code>2 * 10<sup>4</sup></code>.
*   <code>0 <= l <= r <= 2 * 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.min

@Suppress("NAME_SHADOWING")
class Solution {
    private val mod = 1000000007
    private val intMap = IntMap()

    fun countSubMultisets(nums: List<Int>, l: Int, r: Int): Int {
        intMap.clear()
        intMap.add(0)
        var total = 0
        for (num in nums) {
            intMap.add(num)
            total += num
        }
        if (total < l) {
            return 0
        }
        val r = min(r, total)
        val cnt = IntArray(r + 1)
        cnt[0] = intMap.map[0]
        var sum = 0
        for (i in 1 until intMap.size) {
            val value = intMap.vals[i]
            val count = intMap.map[value]
            if (count > 0) {
                sum = min(r, sum + value * count)
                update(cnt, value, count, sum)
            }
        }
        var res = 0
        for (i in l..r) {
            res = (res + cnt[i]) % mod
        }
        return res
    }

    private fun update(cnt: IntArray, n: Int, count: Int, sum: Int) {
        if (count == 1) {
            for (i in sum downTo n) {
                cnt[i] = (cnt[i] + cnt[i - n]) % mod
            }
        } else {
            for (i in n..sum) {
                cnt[i] = (cnt[i] + cnt[i - n]) % mod
            }
            val max = (count + 1) * n
            for (i in sum downTo max) {
                cnt[i] = (cnt[i] - cnt[i - max] + mod) % mod
            }
        }
    }

    private class IntMap {
        private val max = 20001
        val map = IntArray(max)
        val vals = IntArray(max)
        var size = 0

        fun add(v: Int) {
            if (map[v]++ == 0) {
                vals[size++] = v
            }
        }

        fun clear() {
            for (i in 0 until size) {
                map[vals[i]] = 0
            }
            size = 0
        }
    }
}
```