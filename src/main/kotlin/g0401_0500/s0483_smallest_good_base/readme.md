[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 483\. Smallest Good Base

Hard

Given an integer `n` represented as a string, return _the smallest **good base** of_ `n`.

We call `k >= 2` a **good base** of `n`, if all digits of `n` base `k` are `1`'s.

**Example 1:**

**Input:** n = "13"

**Output:** "3"

**Explanation:** 13 base 3 is 111.

**Example 2:**

**Input:** n = "4681"

**Output:** "8"

**Explanation:** 4681 base 8 is 11111.

**Example 3:**

**Input:** n = "1000000000000000000"

**Output:** "999999999999999999"

**Explanation:** 1000000000000000000 base 999999999999999999 is 11.

**Constraints:**

*   `n` is an integer in the range <code>[3, 10<sup>18</sup>]</code>.
*   `n` does not contain any leading zeros.

## Solution

```kotlin
class Solution {
    fun smallestGoodBase(n: String): String {
        return sol1(n)
    }

    private fun sol1(n: String): String {
        val x = n.toLong()
        val ans: MutableList<Long> = ArrayList()
        ans.add(x - 1)
        val y = x - 1
        for (i in 2..62) {
            val dm = Math.pow(y.toDouble(), 1.0 / i)
            val dml = dm.toLong()
            var j = 0
            while (j > -3 && dml + j > 1) {
                val d = dml + j
                if (y % d == 0L) {
                    val poly = poly(d, i)
                    if (poly == x) {
                        ans.add(d)
                    }
                }
                j--
            }
        }
        val end = ans[ans.size - 1]
        return end.toString()
    }

    private fun poly(b: Long, n: Int): Long {
        var ans: Long = 1
        var m: Long = 1
        for (i in 0 until n) {
            m *= b
            ans += m
        }
        return ans
    }
}
```