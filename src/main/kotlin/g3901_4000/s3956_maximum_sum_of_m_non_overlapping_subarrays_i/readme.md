[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3956\. Maximum Sum of M Non-Overlapping Subarrays I

Hard

You are given an integer array `nums` of length `n`, and three integers `m`, `l`, and `r`.

Your task is to select **at least** one and **at most** `m` **non-overlapping non-empty subarrays** from `nums` such that:

*   Each selected **subarray** has a length between `[l, r]` (inclusive).
*   The total sum of all selected **subarrays** is **maximized**.

Return the **maximum** total sum you can achieve.

**Example 1:**

**Input:** nums = [4,1,-5,2], m = 2, l = 1, r = 3

**Output:** 7

**Explanation:**

One optimal strategy is to:

*   Select the subarray `[4, 1]` with sum `4 + 1 = 5` and the subarray `[2]` with sum 2. Both subarrays have length between `[l, r]`.
*   The total sum of these subarrays is `5 + 2 = 7`, which is the maximum achievable sum with at most `m = 2` subarrays.

**Example 2:**

**Input:** nums = [1,0,3,4], m = 2, l = 1, r = 2

**Output:** 8

**Explanation:**

One optimal strategy is to:

*   Select the subarray `[1]` with sum `1` and the subarray `[3, 4]` with sum `3 + 4 = 7`. Both subarrays have length between `[l, r]`.
*   The total sum of these subarrays is `1 + 7 = 8`, which is the maximum achievable sum with at most `m = 2` subarrays.

**Example 3:**

**Input:** nums = [-1,7,-4], m = 1, l = 2, r = 3

**Output:** 6

**Explanation:**

*   Select the subarray `[-1, 7]` from `nums` which has length between `[l, r]`.
*   The total sum of this subarray is `-1 + 7 = 6`, which is the maximum achievable sum with at most `m = 1` subarray.

**Example 4:**

**Input:** nums = [-3,-4,-1], m = 2, l = 1, r = 2

**Output:** \-1

**Explanation:**

*   All subarrays of `nums` have negative sums. The optimal strategy is to select the subarray `[-1]`, which has length between `[l, r]`.
*   The total sum of this subarray is -1, which is the maximum achievable sum with at most `m = 2` subarrays.

**Constraints:**

*   `1 <= n == nums.length <= 1000`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= m <= n`
*   `1 <= l <= r <= n`

## Solution

```kotlin
import kotlin.math.max


class Solution {
    fun maximumSum(nums: IntArray, m: Int, l: Int, r: Int): Long {
        val n = nums.size
        val pre = LongArray(n + 1)
        for (i in 0..<n) {
            pre[i + 1] = pre[i] + nums[i]
        }
        val neg = (-4e18).toLong()
        var prev = LongArray(n + 1)
        for (i in 0..n) {
            prev[i] = 0
        }
        var res = neg
        for (t in 1..m) {
            val curr = LongArray(n + 1)
            for (i in 0..n) {
                curr[i] = neg
            }
            val dq = IntArray(n + 1)
            var head = 0
            var tail = 0
            for (i in 1..n) {
                curr[i] = curr[i - 1]
                val add = i - l
                if (add >= 0 && prev[add] != neg) {
                    val `val` = prev[add] - pre[add]
                    while (head < tail) {
                        val idx = dq[tail - 1]
                        val last = prev[idx] - pre[idx]
                        if (last <= `val`) {
                            tail--
                        } else {
                            break
                        }
                    }
                    dq[tail++] = add
                }
                val left = i - r
                while (head < tail && dq[head] < left) {
                    head++
                }
                if (head < tail) {
                    val s = dq[head]
                    curr[i] = max(curr[i], pre[i] + prev[s] - pre[s])
                }
            }
            res = max(res, curr[n])
            prev = curr
        }
        return res
    }
}
```