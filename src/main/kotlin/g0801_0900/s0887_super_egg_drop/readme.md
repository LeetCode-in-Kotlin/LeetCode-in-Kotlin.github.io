[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 887\. Super Egg Drop

Hard

You are given `k` identical eggs and you have access to a building with `n` floors labeled from `1` to `n`.

You know that there exists a floor `f` where `0 <= f <= n` such that any egg dropped at a floor **higher** than `f` will **break**, and any egg dropped **at or below** floor `f` will **not break**.

Each move, you may take an unbroken egg and drop it from any floor `x` (where `1 <= x <= n`). If the egg breaks, you can no longer use it. However, if the egg does not break, you may **reuse** it in future moves.

Return _the **minimum number of moves** that you need to determine **with certainty** what the value of_ `f` is.

**Example 1:**

**Input:** k = 1, n = 2

**Output:** 2

**Explanation:**  

Drop the egg from floor 1. If it breaks, we know that f = 0. 

Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1. 

If it does not break, then we know f = 2. 

Hence, we need at minimum 2 moves to determine with certainty what the value of f is.

**Example 2:**

**Input:** k = 2, n = 6

**Output:** 3

**Example 3:**

**Input:** k = 3, n = 14

**Output:** 4

**Constraints:**

*   `1 <= k <= 100`
*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun superEggDrop(k: Int, n: Int): Int {
        val dp = IntArray(k + 1)
        var counter = 1
        while (true) {
            var temp = dp[0]
            for (i in 1 until dp.size) {
                val localValue = dp[i] + temp + 1
                temp = dp[i]
                dp[i] = localValue
                if (localValue >= n) {
                    return counter
                }
            }
            counter += 1
        }
    }
}
```