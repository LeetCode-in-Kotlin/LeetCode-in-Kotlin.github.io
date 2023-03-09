[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 746\. Min Cost Climbing Stairs

Easy

You are given an integer array `cost` where `cost[i]` is the cost of <code>i<sup>th</sup></code> step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return _the minimum cost to reach the top of the floor_.

**Example 1:**

**Input:** cost = [10,<ins>15</ins>,20]

**Output:** 15

**Explanation:** You will start at index 1. - Pay 15 and climb two steps to reach the top. The total cost is 15.

**Example 2:**

**Input:** cost = [<ins>1</ins>,100,<ins>1</ins>,1,<ins>1</ins>,100,<ins>1</ins>,<ins>1</ins>,100,<ins>1</ins>]

**Output:** 6

**Explanation:** You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top. The total cost is 6.

**Constraints:**

*   `2 <= cost.length <= 1000`
*   `0 <= cost[i] <= 999`

## Solution

```kotlin
class Solution {
    fun minCostClimbingStairs(cost: IntArray): Int {
        val dp = IntArray(cost.size)
        dp[0] = cost[0]
        dp[1] = cost[1]
        for (i in 2 until cost.size) {
            dp[i] = cost[i] + dp[i - 1].coerceAtMost(dp[i - 2])
        }
        return dp[cost.size - 1].coerceAtMost(dp[cost.size - 2])
    }
}
```