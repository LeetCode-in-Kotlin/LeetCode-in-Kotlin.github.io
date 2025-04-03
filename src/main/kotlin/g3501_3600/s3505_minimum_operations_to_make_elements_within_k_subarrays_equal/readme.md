[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3505\. Minimum Operations to Make Elements Within K Subarrays Equal

Hard

You are given an integer array `nums` and two integers, `x` and `k`. You can perform the following operation any number of times (**including zero**):

*   Increase or decrease any element of `nums` by 1.

Return the **minimum** number of operations needed to have **at least** `k` _non-overlapping **non-empty subarrays**_ of size **exactly** `x` in `nums`, where all elements within each subarray are equal.

**Example 1:**

**Input:** nums = [5,-2,1,3,7,3,6,4,-1], x = 3, k = 2

**Output:** 8

**Explanation:**

*   Use 3 operations to add 3 to `nums[1]` and use 2 operations to subtract 2 from `nums[3]`. The resulting array is `[5, 1, 1, 1, 7, 3, 6, 4, -1]`.
*   Use 1 operation to add 1 to `nums[5]` and use 2 operations to subtract 2 from `nums[6]`. The resulting array is `[5, 1, 1, 1, 7, 4, 4, 4, -1]`.
*   Now, all elements within each subarray `[1, 1, 1]` (from indices 1 to 3) and `[4, 4, 4]` (from indices 5 to 7) are equal. Since 8 total operations were used, 8 is the output.

**Example 2:**

**Input:** nums = [9,-2,-2,-2,1,5], x = 2, k = 2

**Output:** 3

**Explanation:**

*   Use 3 operations to subtract 3 from `nums[4]`. The resulting array is `[9, -2, -2, -2, -2, 5]`.
*   Now, all elements within each subarray `[-2, -2]` (from indices 1 to 2) and `[-2, -2]` (from indices 3 to 4) are equal. Since 3 operations were used, 3 is the output.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>
*   `2 <= x <= nums.length`
*   `1 <= k <= 15`
*   `2 <= k * x <= nums.length`

## Solution

```kotlin
import java.util.Collections
import java.util.PriorityQueue
import kotlin.math.min

class Solution {
    private class SlidingMedian {
        // max-heap for smaller half
        var leftHeap: PriorityQueue<Int>

        // min-heap for larger half
        var rightHeap: PriorityQueue<Int>
        var delayedRemovals: MutableMap<Int, Int>
        var sumLeft: Long
        var sumRight: Long = 0
        var sizeLeft: Int
        var sizeRight: Int

        init {
            leftHeap = PriorityQueue<Int>(Collections.reverseOrder<Int?>())
            rightHeap = PriorityQueue<Int>()
            delayedRemovals = HashMap<Int, Int>()
            sumLeft = sumRight
            sizeRight = 0
            sizeLeft = sizeRight
        }

        fun add(num: Int) {
            if (leftHeap.isEmpty() || num <= leftHeap.peek()!!) {
                leftHeap.offer(num)
                sumLeft += num
                sizeLeft++
            } else {
                rightHeap.offer(num)
                sumRight += num
                sizeRight++
            }
            balanceHeaps()
        }

        fun remove(num: Int) {
            delayedRemovals.put(num, delayedRemovals.getOrDefault(num, 0) + 1)
            if (leftHeap.isNotEmpty() && num <= leftHeap.peek()!!) {
                sumLeft -= num
                sizeLeft--
            } else {
                sumRight -= num
                sizeRight--
            }
            balanceHeaps()
            pruneHeap(leftHeap)
            pruneHeap(rightHeap)
        }

        fun balanceHeaps() {
            if (sizeLeft > sizeRight + 1) {
                val num = leftHeap.poll()!!
                sumLeft -= num
                sizeLeft--
                rightHeap.offer(num)
                sumRight += num
                sizeRight++
            } else if (sizeRight > sizeLeft) {
                val num = rightHeap.poll()!!
                sumRight -= num
                sizeRight--
                leftHeap.offer(num)
                sumLeft += num
                sizeLeft++
            }
        }

        fun pruneHeap(heap: PriorityQueue<Int>) {
            while (heap.isNotEmpty() && delayedRemovals.containsKey(heap.peek())) {
                val num: Int = heap.peek()!!
                if (delayedRemovals[num]!! > 0) {
                    heap.poll()
                    delayedRemovals.put(num, delayedRemovals[num]!! - 1)
                    if (delayedRemovals[num] == 0) {
                        delayedRemovals.remove(num)
                    }
                } else {
                    break
                }
            }
        }

        val median: Int
            get() = leftHeap.peek()!!

        val cost: Long
            get() {
                val median = this.median
                return median * sizeLeft - sumLeft + sumRight - median * sizeRight
            }
    }

    fun minOperations(nums: IntArray, x: Int, k: Int): Long {
        val n = nums.size
        val windowCount = n - x + 1
        val costs = LongArray(windowCount)
        val sm = SlidingMedian()
        // Compute costs for all windows
        for (i in 0..<x) {
            sm.add(nums[i])
        }
        costs[0] = sm.cost
        for (i in 1..<windowCount) {
            sm.remove(nums[i - 1])
            sm.add(nums[i + x - 1])
            costs[i] = sm.cost
        }
        // Dynamic programming table
        val dp: Array<LongArray> = Array<LongArray>(windowCount) { LongArray(k + 1) }
        for (row in dp) {
            row.fill(Long.Companion.MAX_VALUE / 2)
        }
        dp[0][0] = 0
        for (i in 0..<windowCount) {
            for (j in 0..k) {
                if (i > 0) {
                    dp[i][j] = min(dp[i][j], dp[i - 1][j])
                }
                if (j > 0 && i >= x) {
                    dp[i][j] = min(dp[i][j], (dp[i - x][j - 1] + costs[i]))
                } else if (j == 1) {
                    dp[i][j] = min(dp[i][j], costs[i])
                }
            }
        }
        return dp[windowCount - 1][k]
    }
}
```