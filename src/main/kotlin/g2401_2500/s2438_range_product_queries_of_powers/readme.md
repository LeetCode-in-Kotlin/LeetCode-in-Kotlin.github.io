[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2438\. Range Product Queries of Powers

Medium

Given a positive integer `n`, there exists a **0-indexed** array called `powers`, composed of the **minimum** number of powers of `2` that sum to `n`. The array is sorted in **non-decreasing** order, and there is **only one** way to form the array.

You are also given a **0-indexed** 2D integer array `queries`, where <code>queries[i] = [left<sub>i</sub>, right<sub>i</sub>]</code>. Each `queries[i]` represents a query where you have to find the product of all `powers[j]` with <code>left<sub>i</sub> <= j <= right<sub>i</sub></code>.

Return _an array_ `answers`_, equal in length to_ `queries`_, where_ `answers[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_. Since the answer to the <code>i<sup>th</sup></code> query may be too large, each `answers[i]` should be returned **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 15, queries = \[\[0,1],[2,2],[0,3]]

**Output:** [2,4,64]

**Explanation:** 

For n = 15, powers = [1,2,4,8]. It can be shown that powers cannot be a smaller size. 

Answer to 1st query: powers[0] * powers[1] = 1 * 2 = 2. 

Answer to 2nd query: powers[2] = 4. 

Answer to 3rd query: powers[0] * powers[1] * powers[2] * powers[3] = 1 * 2 * 4 * 8 = 64. 

Each answer modulo 10<sup>9</sup> + 7 yields the same answer, so [2,4,64] is returned.

**Example 2:**

**Input:** n = 2, queries = \[\[0,0]]

**Output:** [2]

**Explanation:** For n = 2, powers = [2]. The answer to the only query is powers[0] = 2. The answer modulo 10<sup>9</sup> + 7 is the same, so [2] is returned.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> < powers.length</code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun productQueries(n: Int, queries: Array<IntArray>): IntArray {
        val length = queries.size
        val mod = (1e9 + 7).toLong()
        // convert n to binary form
        // take the set bit and find the corresponding 2^i
        // now answer for the query
        val powerTracker = IntArray(32)
        val productTakingPowers: MutableList<Long> = ArrayList()
        val result = IntArray(length)
        fillPowerTracker(powerTracker, n)
        fillProductTakingPowers(productTakingPowers, powerTracker)
        var index = 0
        for (query in queries) {
            val left = query[0]
            val right = query[1]
            var product: Long = 1
            for (i in left..right) {
                product = product * productTakingPowers[i] % mod
            }
            result[index++] = (product % mod).toInt()
        }
        return result
    }

    private fun fillPowerTracker(powerTracker: IntArray, n: Int) {
        var n = n
        var index = 0
        while (n > 0) {
            powerTracker[index++] = n and 1
            n = n shr 1
        }
    }

    private fun fillProductTakingPowers(productTakingPowers: MutableList<Long>, powerTracker: IntArray) {
        for (i in 0..31) {
            if (powerTracker[i] == 1) {
                val power = Math.pow(2.0, i.toDouble()).toLong()
                productTakingPowers.add(power)
            }
        }
    }
}
```