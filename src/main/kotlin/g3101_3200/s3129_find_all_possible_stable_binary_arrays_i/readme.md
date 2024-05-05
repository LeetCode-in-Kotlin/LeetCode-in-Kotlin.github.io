[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3129\. Find All Possible Stable Binary Arrays I

Medium

You are given 3 positive integers `zero`, `one`, and `limit`.

A binary array `arr` is called **stable** if:

*   The number of occurrences of 0 in `arr` is **exactly** `zero`.
*   The number of occurrences of 1 in `arr` is **exactly** `one`.
*   Each subarray of `arr` with a size greater than `limit` must contain **both** 0 and 1.

Return the _total_ number of **stable** binary arrays.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** zero = 1, one = 1, limit = 2

**Output:** 2

**Explanation:**

The two possible stable binary arrays are `[1,0]` and `[0,1]`, as both arrays have a single 0 and a single 1, and no subarray has a length greater than 2.

**Example 2:**

**Input:** zero = 1, one = 2, limit = 1

**Output:** 1

**Explanation:**

The only possible stable binary array is `[1,0,1]`.

Note that the binary arrays `[1,1,0]` and `[0,1,1]` have subarrays of length 2 with identical elements, hence, they are not stable.

**Example 3:**

**Input:** zero = 3, one = 3, limit = 2

**Output:** 14

**Explanation:**

All the possible stable binary arrays are `[0,0,1,0,1,1]`, `[0,0,1,1,0,1]`, `[0,1,0,0,1,1]`, `[0,1,0,1,0,1]`, `[0,1,0,1,1,0]`, `[0,1,1,0,0,1]`, `[0,1,1,0,1,0]`, `[1,0,0,1,0,1]`, `[1,0,0,1,1,0]`, `[1,0,1,0,0,1]`, `[1,0,1,0,1,0]`, `[1,0,1,1,0,0]`, `[1,1,0,0,1,0]`, and `[1,1,0,1,0,0]`.

**Constraints:**

*   `1 <= zero, one, limit <= 200`

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.max
import kotlin.math.min

class Solution {
    private fun add(x: Int, y: Int): Int {
        return (x + y) % MODULUS
    }

    private fun subtract(x: Int, y: Int): Int {
        return (x + MODULUS - y) % MODULUS
    }

    private fun multiply(x: Int, y: Int): Int {
        return (x.toLong() * y % MODULUS).toInt()
    }

    fun numberOfStableArrays(zero: Int, one: Int, limit: Int): Int {
        if (limit == 1) {
            return max((2 - abs((zero - one))), 0)
        }
        val max = max(zero, one)
        val min = min(zero, one)
        val lcn = Array(max + 1) { IntArray(max + 1) }
        var row0 = lcn[0]
        var row1: IntArray
        var row2: IntArray
        row0[0] = 1
        var s = 1
        var sLim = s - limit
        while (s <= max) {
            row2 = if (sLim > 0) lcn[sLim - 1] else intArrayOf()
            row1 = row0
            row0 = lcn[s]
            var c = (s - 1) / limit + 1
            while (c <= sLim) {
                row0[c] = subtract(add(row1[c], row1[c - 1]), row2[c - 1])
                c++
            }
            while (c <= s) {
                row0[c] = add(row1[c], row1[c - 1])
                c++
            }
            s++
            sLim++
        }
        row1 = lcn[min]
        var result = 0
        var s0 = add(if (min < max) row0[min + 1] else 0, row0[min])
        for (c in min downTo 1) {
            val s1 = s0
            s0 = add(row0[c], row0[c - 1])
            result = add(result, multiply(row1[c], add(s0, s1)))
        }
        return result
    }

    companion object {
        private const val MODULUS = 1e9.toInt() + 7
    }
}
```