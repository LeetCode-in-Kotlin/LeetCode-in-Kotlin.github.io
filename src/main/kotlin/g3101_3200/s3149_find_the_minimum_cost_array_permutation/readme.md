[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3149\. Find the Minimum Cost Array Permutation

Hard

You are given an array `nums` which is a permutation of `[0, 1, 2, ..., n - 1]`. The **score** of any permutation of `[0, 1, 2, ..., n - 1]` named `perm` is defined as:

`score(perm) = |perm[0] - nums[perm[1]]| + |perm[1] - nums[perm[2]]| + ... + |perm[n - 1] - nums[perm[0]]|`

Return the permutation `perm` which has the **minimum** possible score. If _multiple_ permutations exist with this score, return the one that is lexicographically smallest among them.

**Example 1:**

**Input:** nums = [1,0,2]

**Output:** [0,1,2]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/04/example0gif.gif)**

The lexicographically smallest permutation with minimum cost is `[0,1,2]`. The cost of this permutation is `|0 - 0| + |1 - 2| + |2 - 1| = 2`.

**Example 2:**

**Input:** nums = [0,2,1]

**Output:** [0,2,1]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/04/example1gif.gif)**

The lexicographically smallest permutation with minimum cost is `[0,2,1]`. The cost of this permutation is `|0 - 1| + |2 - 2| + |1 - 0| = 2`.

**Constraints:**

*   `2 <= n == nums.length <= 14`
*   `nums` is a permutation of `[0, 1, 2, ..., n - 1]`.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.min

class Solution {
    private fun findMinScore(mask: Int, prevNum: Int, nums: IntArray, dp: Array<IntArray>): Int {
        val n = nums.size
        if (Integer.bitCount(mask) == n) {
            dp[mask][prevNum] = abs((prevNum - nums[0]))
            return dp[mask][prevNum]
        }
        if (dp[mask][prevNum] != -1) {
            return dp[mask][prevNum]
        }
        var minScore = Int.MAX_VALUE
        for (currNum in 0 until n) {
            if ((mask shr currNum and 1 xor 1) == 1) {
                val currScore = (
                    abs((prevNum - nums[currNum])) + findMinScore(
                        mask or (1 shl currNum),
                        currNum,
                        nums,
                        dp,
                    )
                    )
                minScore = min(minScore, currScore)
            }
        }
        return minScore.also { dp[mask][prevNum] = it }
    }

    private fun constructMinScorePermutation(n: Int, nums: IntArray, dp: Array<IntArray>): IntArray {
        val permutation = IntArray(n)
        var i = 0
        permutation[i++] = 0
        var prevNum = 0
        var mask = 1
        while (i < n) {
            for (currNum in 0 until n) {
                if ((mask shr currNum and 1 xor 1) == 1) {
                    val currScore =
                        (abs((prevNum - nums[currNum])) + dp[mask or (1 shl currNum)][currNum])
                    val minScore = dp[mask][prevNum]
                    if (currScore == minScore) {
                        permutation[i++] = currNum
                        prevNum = currNum
                        break
                    }
                }
            }
            mask = mask or (1 shl prevNum)
        }
        return permutation
    }

    fun findPermutation(nums: IntArray): IntArray {
        val n = nums.size
        val dp = Array(1 shl n) { IntArray(n) }
        for (row in dp) {
            row.fill(-1)
        }
        findMinScore(1, 0, nums, dp)
        return constructMinScorePermutation(n, nums, dp)
    }
}
```