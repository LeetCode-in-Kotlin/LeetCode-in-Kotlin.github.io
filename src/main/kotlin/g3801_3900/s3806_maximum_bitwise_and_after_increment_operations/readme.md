[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3806\. Maximum Bitwise AND After Increment Operations

Hard

You are given an integer array `nums` and two integers `k` and `m`.

You may perform **at most** `k` operations. In one operation, you may choose any index `i` and **increase** `nums[i]` by 1.

Return an integer denoting the **maximum** possible **bitwise AND** of any **subset** of size `m` after performing up to `k` operations optimally.

**Example 1:**

**Input:** nums = [3,1,2], k = 8, m = 2

**Output:** 6

**Explanation:**

*   We need a subset of size `m = 2`. Choose indices `[0, 2]`.
*   Increase `nums[0] = 3` to 6 using 3 operations, and increase `nums[2] = 2` to 6 using 4 operations.
*   The total number of operations used is 7, which is not greater than `k = 8`.
*   The two chosen values become `[6, 6]`, and their bitwise AND is `6`, which is the maximum possible.

**Example 2:**

**Input:** nums = [1,2,8,4], k = 7, m = 3

**Output:** 4

**Explanation:**

*   We need a subset of size `m = 3`. Choose indices `[0, 1, 3]`.
*   Increase `nums[0] = 1` to 4 using 3 operations, increase `nums[1] = 2` to 4 using 2 operations, and keep `nums[3] = 4`.
*   The total number of operations used is 5, which is not greater than `k = 7`.
*   The three chosen values become `[4, 4, 4]`, and their bitwise AND is 4, which is the maximum possible.

**Example 3:**

**Input:** nums = [1,1], k = 3, m = 2

**Output:** 2

**Explanation:**

*   We need a subset of size `m = 2`. Choose indices `[0, 1]`.
*   Increase both values from 1 to 2 using 1 operation each.
*   The total number of operations used is 2, which is not greater than `k = 3`.
*   The two chosen values become `[2, 2]`, and their bitwise AND is 2, which is the maximum possible.

**Constraints:**

*   <code>1 <= n == nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>
*   `1 <= m <= n`

## Solution

```kotlin
class Solution {
    fun maximumAND(a: IntArray, b: Int, c: Int): Int {
        var e: Long = 0L
        val f = a.size
        val g = LongArray(f)
        for (h in 30 downTo 0) {
            val i = e or (1L shl h)
            for (j in 0..<f) {
                val k = a[j].toLong()
                val l = i and k.inv()
                if (l == 0L) {
                    g[j] = 0
                } else {
                    var n = 63 - java.lang.Long.numberOfLeadingZeros(l)
                    while (((k shr n) and 1L) == 1L) {
                        n++
                    }
                    val o = (1L shl n) - 1
                    g[j] = ((k and o.inv()) or (1L shl n) or (i and o)) - k
                }
            }
            g.sort()
            var p: Long = 0
            for (q in 0..<c) {
                p += g[q]
            }
            if (p <= b) {
                e = i
            }
        }
        return e.toInt()
    }
}
```