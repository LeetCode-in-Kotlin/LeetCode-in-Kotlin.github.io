[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3892\. Minimum Operations to Achieve At Least K Peaks

Hard

You are given a circular integer array `nums` of length `n`.

An index `i` is a **peak** if its value is **strictly greater** than its neighbors:

*   The **previous** neighbor of `i` is `nums[i - 1]` if `i > 0`, otherwise `nums[n - 1]`.
*   The **next** neighbor of `i` is `nums[i + 1]` if `i < n - 1`, otherwise `nums[0]`.

You are allowed to perform the following operation **any** number of times:

*   Choose any index `i` and **increase** `nums[i]` by 1.

Return an integer denoting the **minimum** number of operations required to make the array contain **at least** `k` peaks. If it is impossible, return -1.

**Example 1:**

**Input:** nums = [2,1,2], k = 1

**Output:** 1

**Explanation:**

*   To achieve at least `k = 1` peak, we can increase `nums[2] = 2` to 3.
*   After this operation, `nums[2] = 3` is strictly greater than its neighbors `nums[0] = 2` and `nums[1] = 1`.
*   Therefore, the minimum number of operations required is 1.

**Example 2:**

**Input:** nums = [4,5,3,6], k = 2

**Output:** 0

**Explanation:**

*   The array already contains at least `k = 2` peaks with zero operations.
*   Index 1: `nums[1] = 5` is strictly greater than its neighbors `nums[0] = 4` and `nums[2] = 3`.
*   Index 3: `nums[3] = 6` is strictly greater than its neighbors `nums[2] = 3` and `nums[0] = 4`.
*   Therefore, the minimum number of operations required is 0.

**Example 3:**

**Input:** nums = [3,7,3], k = 2

**Output:** \-1

**Explanation:**

It is impossible to have at least `k = 2` peaks in this array. Therefore, the answer is -1.

**Constraints:**

*   `2 <= n == nums.length <= 5000`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `0 <= k <= n`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minOperations(nums: IntArray, k: Int): Int {
        val n = nums.size
        if (k == 0) {
            return 0
        }
        if (k > n / 2) {
            return -1
        }
        val c = LongArray(n)
        for (i in 0..<n) {
            val left = nums[(i - 1 + n) % n]
            val right = nums[(i + 1) % n]
            c[i] = max(0L, max(left, right).toLong() + 1 - nums[i])
        }
        var best = pathMin(c, 1, n - 1, k)
        val withZero = pathMin(c, 2, n - 2, k - 1)
        if (withZero < INF) {
            best = min(best, c[0] + withZero)
        }
        return best.toInt()
    }

    private fun pathMin(c: LongArray, lo: Int, hi: Int, need: Int): Long {
        if (need == 0) {
            return 0
        }
        val m = hi - lo + 1
        if (m <= 0 || need > (m + 1) / 2) {
            return INF
        }
        var prev2 = LongArray(need + 1)
        var prev1 = LongArray(need + 1)
        var cur = LongArray(need + 1)
        prev2.fill(INF)
        prev2[0] = 0
        prev1.fill(INF)
        prev1[0] = 0
        for (i in 0..<m) {
            val w = c[lo + i]
            cur[0] = 0
            for (j in 1..need) {
                cur[j] = min(prev1[j], prev2[j - 1] + w)
            }
            val t = prev2
            prev2 = prev1
            prev1 = cur
            cur = t
        }
        return prev1[need]
    }

    companion object {
        private val INF = Long.MAX_VALUE / 4
    }
}
```