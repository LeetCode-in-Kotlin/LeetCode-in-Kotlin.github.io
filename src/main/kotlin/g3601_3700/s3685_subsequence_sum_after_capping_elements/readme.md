[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3685\. Subsequence Sum After Capping Elements

Medium

You are given an integer array `nums` of size `n` and a positive integer `k`.

An array **capped** by value `x` is obtained by replacing every element `nums[i]` with `min(nums[i], x)`.

For each integer `x` from 1 to `n`, determine whether it is possible to choose a **subsequence** from the array capped by `x` such that the sum of the chosen elements is **exactly** `k`.

Return a **0-indexed** boolean array `answer` of size `n`, where `answer[i]` is `true` if it is possible when using `x = i + 1`, and `false` otherwise.

**Example 1:**

**Input:** nums = [4,3,2,4], k = 5

**Output:** [false,false,true,true]

**Explanation:**

*   For `x = 1`, the capped array is `[1, 1, 1, 1]`. Possible sums are `1, 2, 3, 4`, so it is impossible to form a sum of `5`.
*   For `x = 2`, the capped array is `[2, 2, 2, 2]`. Possible sums are `2, 4, 6, 8`, so it is impossible to form a sum of `5`.
*   For `x = 3`, the capped array is `[3, 3, 2, 3]`. A subsequence `[2, 3]` sums to `5`, so it is possible.
*   For `x = 4`, the capped array is `[4, 3, 2, 4]`. A subsequence `[3, 2]` sums to `5`, so it is possible.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 3

**Output:** [true,true,true,true,true]

**Explanation:**

For every value of `x`, it is always possible to select a subsequence from the capped array that sums exactly to `3`.

**Constraints:**

*   `1 <= n == nums.length <= 4000`
*   `1 <= nums[i] <= n`
*   `1 <= k <= 4000`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun subsequenceSumAfterCapping(nums: IntArray, k: Int): BooleanArray {
        val zolvarinte = nums
        val n = zolvarinte.size
        val answer = BooleanArray(n)
        val maxV = n
        val freq = IntArray(maxV + 2)
        for (v in zolvarinte) {
            if (v <= maxV) {
                freq[v]++
            }
        }
        val cntGe = IntArray(maxV + 2)
        cntGe[maxV] = freq[maxV]
        for (x in maxV - 1 downTo 1) {
            cntGe[x] = cntGe[x + 1] + freq[x]
        }
        val dp = BooleanArray(k + 1)
        dp[0] = true
        for (x in 1..n) {
            val cnt = cntGe[x]
            var ok = false
            var maxM = cnt
            val limit = k / x
            if (maxM > limit) {
                maxM = limit
            }
            for (m in 0..maxM) {
                val rem = k - m * x
                if (rem >= 0 && dp[rem]) {
                    ok = true
                    break
                }
            }
            answer[x - 1] = ok
            var c = freq[x]
            if (c == 0) {
                continue
            }
            var power = 1
            while (c > 0) {
                val take = min(power, c)
                val weight = take * x
                for (s in k downTo weight) {
                    if (!dp[s] && dp[s - weight]) {
                        dp[s] = true
                    }
                }
                c -= take
                power = power shl 1
            }
        }
        return answer
    }
}
```