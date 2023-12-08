[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2818\. Apply Operations to Maximize Score

Hard

You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

*   Choose any **non-empty** subarray `nums[l, ..., r]` that you haven't chosen previously.
*   Choose an element `x` of `nums[l, ..., r]` with the highest **prime score**. If multiple such elements exist, choose the one with the smallest index.
*   Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The **prime score** of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return _the **maximum possible score** after applying at most_ `k` _operations_.

Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [8,3,9,3,8], k = 2

**Output:** 81

**Explanation:**

To get a score of 81, we can apply the following operations:

- Choose subarray nums[2, ..., 2]. nums[2] is the only element in this subarray. Hence, we multiply the score by nums[2]. The score becomes 1 \* 9 = 9.

- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 1, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 9 \* 9 = 81.

It can be proven that 81 is the highest score one can obtain.

**Example 2:**

**Input:** nums = [19,12,14,6,10,18], k = 3

**Output:** 4788

**Explanation:**

To get a score of 4788, we can apply the following operations:

- Choose subarray nums[0, ..., 0]. nums[0] is the only element in this subarray. Hence, we multiply the score by nums[0]. The score becomes 1 \* 19 = 19.

- Choose subarray nums[5, ..., 5]. nums[5] is the only element in this subarray. Hence, we multiply the score by nums[5]. The score becomes 19 \* 18 = 342.

- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 2, but nums[2] has the smaller index. Hence, we multipy the score by nums[2]. The score becomes 342 \* 14 = 4788. It can be proven that 4788 is the highest score one can obtain. 

**Constraints:**

*   <code>1 <= nums.length == n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= min(n * (n + 1) / 2, 10<sup>9</sup>)</code>

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque
import java.util.PriorityQueue
import kotlin.math.min

@Suppress("NAME_SHADOWING")
class Solution {
    fun maximumScore(nums: List<Int?>, k: Int): Int {
        // count strictly using nums.get(i) as the selected num
        var k = k
        val dp = IntArray(nums.size)
        // [val, index]
        val pq = PriorityQueue { o1: IntArray, o2: IntArray ->
            Integer.compare(
                o2[0], o1[0]
            )
        }
        val monoStack: Deque<IntArray> = ArrayDeque()
        dp.fill(1)
        for (i in 0..nums.size) {
            var score = Int.MAX_VALUE
            if (i < nums.size) {
                score = PRIME_SCORES[nums[i]!!]
            }
            // when an element is poped, its right bound is confirmed: (i - left + 1) * (right - i +
            // 1)
            while (monoStack.isNotEmpty() && monoStack.peekFirst()[0] < score) {
                val popIndex = monoStack.pollFirst()[1]
                val actualRightIndexOfPopedElement = i - 1
                dp[popIndex] *= actualRightIndexOfPopedElement - popIndex + 1
            }
            // when an element is pushed, its left bound is confirmed: (i - left + 1) * (right - i +
            // 1)
            if (i < nums.size) {
                val peekIndex = if (monoStack.isEmpty()) -1 else monoStack.peekFirst()[1]
                val actualLeftIndexOfCurrentElement = peekIndex + 1
                dp[i] *= i - actualLeftIndexOfCurrentElement + 1
                monoStack.offerFirst(intArrayOf(score, i))
                pq.offer(intArrayOf(nums[i]!!, i))
            }
        }
        var result: Long = 1
        while (k > 0) {
            val pair = pq.poll()
            val `val` = pair[0]
            val index = pair[1]
            val times = min(k.toDouble(), dp[index].toDouble()).toInt()
            val power = pow(`val`.toLong(), times)
            result *= power
            result %= MOD.toLong()
            k -= times
        }
        return result.toInt()
    }

    private fun pow(`val`: Long, times: Int): Long {
        if (times == 1) {
            return `val` % MOD
        }
        val subProblemRes = pow(`val`, times / 2)
        var third = 1L
        if (times % 2 == 1) {
            third = `val`
        }
        return subProblemRes * subProblemRes % MOD * third % MOD
    }

    companion object {
        private const val N = 100000
        private val PRIME_SCORES = computePrimeScores()
        private const val MOD = 1000000000 + 7
        private fun computePrimeScores(): IntArray {
            val primeCnt = IntArray(N + 1)
            for (i in 2..N) {
                if (primeCnt[i] == 0) {
                    var j = i
                    while (j <= N) {
                        primeCnt[j]++
                        j += i
                    }
                }
            }
            return primeCnt
        }
    }
}
```