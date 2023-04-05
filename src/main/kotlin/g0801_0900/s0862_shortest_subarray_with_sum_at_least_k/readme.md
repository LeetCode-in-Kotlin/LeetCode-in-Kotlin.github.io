[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 862\. Shortest Subarray with Sum at Least K

Hard

Given an integer array `nums` and an integer `k`, return _the length of the shortest non-empty **subarray** of_ `nums` _with a sum of at least_ `k`. If there is no such **subarray**, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [1], k = 1

**Output:** 1

**Example 2:**

**Input:** nums = [1,2], k = 4

**Output:** -1

**Example 3:**

**Input:** nums = [2,-1,2], k = 3

**Output:** 3

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    internal class Pair(var index: Int, var value: Long)

    fun shortestSubarray(nums: IntArray, k: Int): Int {
        val n = nums.size
        val dq: Deque<Pair> = LinkedList()
        var ans = Int.MAX_VALUE
        var sum: Long = 0
        for (i in 0 until n) {
            sum += nums[i].toLong()
            // Keep dq in incrementing order
            while (!dq.isEmpty() && sum <= dq.peekLast().value) dq.removeLast()
            // Add current sum and index
            dq.add(Pair(i, sum))
            // Calculate your answer here
            if (sum >= k) ans = Math.min(ans, i + 1)

            // Check if Contraction is possible or not
            while (!dq.isEmpty() && sum - dq.peekFirst().value >= k) {
                ans = ans.coerceAtMost(i - dq.peekFirst().index)
                dq.removeFirst()
            }
        }
        return if (ans == Int.MAX_VALUE) -1 else ans
    }
}
```