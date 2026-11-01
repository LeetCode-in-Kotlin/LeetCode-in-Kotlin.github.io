[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3826\. Minimum Partition Score

Hard

You are given an integer array `nums` and an integer `k`.

Your task is to partition `nums` into **exactly** `k` non-empty subarrays and return an integer denoting the **minimum possible score** among all valid partitions.

The **score** of a partition is the **sum** of the **values** of all its subarrays.

The **value** of a subarray is defined as `sumArr * (sumArr + 1) / 2`, where `sumArr` is the sum of its elements.

**Example 1:**

**Input:** nums = [5,1,2,1], k = 2

**Output:** 25

**Explanation:**

*   We must partition the array into `k = 2` subarrays. One optimal partition is `[5]` and `[1, 2, 1]`.
*   The first subarray has `sumArr = 5` and `value = 5 × 6 / 2 = 15`.
*   The second subarray has `sumArr = 1 + 2 + 1 = 4` and `value = 4 × 5 / 2 = 10`.
*   The score of this partition is `15 + 10 = 25`, which is the minimum possible score.

**Example 2:**

**Input:** nums = [1,2,3,4], k = 1

**Output:** 55

**Explanation:**

*   Since we must partition the array into `k = 1` subarray, all elements belong to the same subarray: `[1, 2, 3, 4]`.
*   This subarray has `sumArr = 1 + 2 + 3 + 4 = 10` and `value = 10 × 11 / 2 = 55`.
*   The score of this partition is 55, which is the minimum possible score.

**Example 3:**

**Input:** nums = [1,1,1], k = 3

**Output:** 3

**Explanation:**

*   We must partition the array into `k = 3` subarrays. The only valid partition is `[1], [1], [1]`.
*   Each subarray has `sumArr = 1` and `value = 1 × 2 / 2 = 1`.
*   The score of this partition is `1 + 1 + 1 = 3`, which is the minimum possible score.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= nums.length`

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

class Solution {
    private lateinit var prefix: LongArray

    private fun isRedundant(l1: Line, l2: Line, l3: Line): Boolean = (
        (l3.intercept - l1.intercept).toDouble() * (l1.slope - l2.slope)
            <= (l2.intercept - l1.intercept).toDouble() * (l1.slope - l3.slope)
        )

    private fun solveWithPenalty(lambda: Long): Pair {
        val dq: Deque<Line> = ArrayDeque<Line>()
        dq.addLast(Line(0, 0, 0))
        var di: Long = 0
        var pi = 0
        for (i in 1..<prefix.size) {
            val x = prefix[i]
            // query
            while (dq.size > 1) {
                val first = dq.pollFirst()
                val second = dq.peekFirst()
                if (first.eval(x) <= second.eval(x)) {
                    dq.addFirst(first)
                    break
                }
            }
            val best = dq.peekFirst()
            di = x * x + x + 2 * lambda + best.eval(x)
            pi = best.partition + 1
            val newLine = Line(-2 * x, di + x * x - x, pi)
            while (dq.size > 1) {
                val last = dq.pollLast()
                val secondLast = dq.peekLast()
                if (!isRedundant(secondLast, last, newLine)) {
                    dq.addLast(last)
                    break
                }
            }
            dq.addLast(newLine)
        }
        return Pair(di, pi)
    }

    fun minPartitionScore(nums: IntArray, k: Int): Long {
        val n = nums.size
        prefix = LongArray(n + 1)
        for (i in 1..n) {
            prefix[i] = prefix[i - 1] + nums[i - 1]
        }
        var start: Long = 0
        var end = 1e15.toLong()
        var bestLambda: Long = 0
        while (start <= end) {
            val mid = start + (end - start) / 2
            val res = solveWithPenalty(mid)
            if (res.partition <= k) {
                bestLambda = mid
                end = mid - 1
            } else {
                start = mid + 1
            }
        }
        val ans = solveWithPenalty(bestLambda)
        return (ans.score - 2L * k * bestLambda) / 2
    }

    private class Line(var slope: Long, var intercept: Long, var partition: Int) {
        fun eval(x: Long): Long = slope * x + intercept
    }

    private class Pair(var score: Long, var partition: Int)
}
```