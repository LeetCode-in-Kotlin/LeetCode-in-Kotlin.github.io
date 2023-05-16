[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1000\. Minimum Cost to Merge Stones

Hard

There are `n` piles of `stones` arranged in a row. The <code>i<sup>th</sup></code> pile has `stones[i]` stones.

A move consists of merging exactly `k` **consecutive** piles into one pile, and the cost of this move is equal to the total number of stones in these `k` piles.

Return _the minimum cost to merge all piles of stones into one pile_. If it is impossible, return `-1`.

**Example 1:**

**Input:** stones = [3,2,4,1], k = 2

**Output:** 20

**Explanation:**

We start with [3, 2, 4, 1]. 

We merge [3, 2] for a cost of 5, and we are left with [5, 4, 1].

We merge [4, 1] for a cost of 5, and we are left with [5, 5]. 

We merge [5, 5] for a cost of 10, and we are left with [10]. 

The total cost was 20, and this is the minimum possible.

**Example 2:**

**Input:** stones = [3,2,4,1], k = 3

**Output:** -1

**Explanation:** After any merge operation, there are 2 piles left, and we can't merge anymore. So the task is impossible.

**Example 3:**

**Input:** stones = [3,5,1,2,6], k = 3

**Output:** 25

**Explanation:** 

We start with [3, 5, 1, 2, 6].

We merge [5, 1, 2] for a cost of 8, and we are left with [3, 8, 6]. 

We merge [3, 8, 6] for a cost of 17, and we are left with [17]. 

The total cost was 25, and this is the minimum possible.

**Constraints:**

*   `n == stones.length`
*   `1 <= n <= 30`
*   `1 <= stones[i] <= 100`
*   `2 <= k <= 30`

## Solution

```kotlin
class Solution {
    private lateinit var memo: Array<IntArray>
    private lateinit var prefixSum: IntArray
    fun mergeStones(stones: IntArray, k: Int): Int {
        val n = stones.size
        if ((n - 1) % (k - 1) != 0) {
            return -1
        }
        memo = Array(n) { IntArray(n) }
        for (arr in memo) {
            arr.fill(-1)
        }
        prefixSum = IntArray(n + 1)
        for (i in 1 until n + 1) {
            prefixSum[i] = prefixSum[i - 1] + stones[i - 1]
        }
        return dp(0, n - 1, k)
    }

    private fun dp(left: Int, right: Int, k: Int): Int {
        if (memo[left][right] > 0) {
            return memo[left][right]
        }
        if (right - left + 1 < k) {
            memo[left][right] = 0
            return memo[left][right]
        }
        if (right - left + 1 == k) {
            memo[left][right] = prefixSum[right + 1] - prefixSum[left]
            return memo[left][right]
        }
        var `val` = Int.MAX_VALUE
        var i = 0
        while (left + i + 1 <= right) {
            `val` = `val`.coerceAtMost(dp(left, left + i, k) + dp(left + i + 1, right, k))
            i += k - 1
        }
        if ((right - left) % (k - 1) == 0) {
            `val` += prefixSum[right + 1] - prefixSum[left]
        }
        memo[left][right] = `val`
        return `val`
    }
}
```