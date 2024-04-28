[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3117\. Minimum Sum of Values by Dividing Array

Hard

You are given two arrays `nums` and `andValues` of length `n` and `m` respectively.

The **value** of an array is equal to the **last** element of that array.

You have to divide `nums` into `m` **disjoint contiguous** subarrays such that for the <code>i<sup>th</sup></code> subarray <code>[l<sub>i</sub>, r<sub>i</sub>]</code>, the bitwise `AND` of the subarray elements is equal to `andValues[i]`, in other words, <code>nums[l<sub>i</sub>] & nums[l<sub>i</sub> + 1] & ... & nums[r<sub>i</sub>] == andValues[i]</code> for all `1 <= i <= m`, where `&` represents the bitwise `AND` operator.

Return _the **minimum** possible sum of the **values** of the_ `m` _subarrays_ `nums` _is divided into_. _If it is not possible to divide_ `nums` _into_ `m` _subarrays satisfying these conditions, return_ `-1`.

**Example 1:**

**Input:** nums = [1,4,3,3,2], andValues = [0,3,3,2]

**Output:** 12

**Explanation:**

The only possible way to divide `nums` is:

1.  `[1,4]` as `1 & 4 == 0`.
2.  `[3]` as the bitwise `AND` of a single element subarray is that element itself.
3.  `[3]` as the bitwise `AND` of a single element subarray is that element itself.
4.  `[2]` as the bitwise `AND` of a single element subarray is that element itself.

The sum of the values for these subarrays is `4 + 3 + 3 + 2 = 12`.

**Example 2:**

**Input:** nums = [2,3,5,7,7,7,5], andValues = [0,7,5]

**Output:** 17

**Explanation:**

There are three ways to divide `nums`:

1.  `[[2,3,5],[7,7,7],[5]]` with the sum of the values `5 + 7 + 5 == 17`.
2.  `[[2,3,5,7],[7,7],[5]]` with the sum of the values `7 + 7 + 5 == 19`.
3.  `[[2,3,5,7,7],[7],[5]]` with the sum of the values `7 + 7 + 5 == 19`.

The minimum possible sum of the values is `17`.

**Example 3:**

**Input:** nums = [1,2,3,4], andValues = [2]

**Output:** \-1

**Explanation:**

The bitwise `AND` of the entire array `nums` is `0`. As there is no possible way to divide `nums` into a single subarray to have the bitwise `AND` of elements `2`, return `-1`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>4</sup></code>
*   `1 <= m == andValues.length <= min(n, 10)`
*   <code>1 <= nums[i] < 10<sup>5</sup></code>
*   <code>0 <= andValues[j] < 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumValueSum(nums: IntArray, andValues: IntArray): Int {
        val n = nums.size
        var dp = IntArray(n + 1)
        dp.fill(INF)
        dp[0] = 0
        for (target in andValues) {
            var sum = INF
            var minSum = INF
            var rightSum = INF
            val leftSum = IntArray(n + 1)
            leftSum[0] = INF
            var left = 0
            var right = 0
            val nextdp = IntArray(n + 1)
            nextdp[0] = INF
            for (i in 0 until n) {
                sum = sum and nums[i]
                rightSum = rightSum and nums[i]
                ++right
                if (sum < target) {
                    minSum = INF
                    sum = nums[i]
                }
                while ((leftSum[left] and rightSum) <= target) {
                    if ((leftSum[left] and rightSum) == target) {
                        minSum = min(minSum, dp[i - left - right + 1])
                    }
                    if (left-- > 0) {
                        continue
                    }
                    left = right
                    var start = i
                    for (l in 1..left) {
                        leftSum[l] = leftSum[l - 1] and nums[start--]
                    }
                    right = 0
                    rightSum = INF
                }
                nextdp[i + 1] = minSum + nums[i]
            }
            dp = nextdp
        }
        return if (dp[n] < INF) dp[n] else -1
    }

    companion object {
        private const val INF = 0xfffffff
    }
}
```