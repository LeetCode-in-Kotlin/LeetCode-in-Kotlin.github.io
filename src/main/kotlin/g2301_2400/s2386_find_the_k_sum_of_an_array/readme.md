[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2386\. Find the K-Sum of an Array

Hard

You are given an integer array `nums` and a **positive** integer `k`. You can choose any **subsequence** of the array and sum all of its elements together.

We define the **K-Sum** of the array as the <code>k<sup>th</sup></code> **largest** subsequence sum that can be obtained (**not** necessarily distinct).

Return _the K-Sum of the array_.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Note** that the empty subsequence is considered to have a sum of `0`.

**Example 1:**

**Input:** nums = [2,4,-2], k = 5

**Output:** 2

**Explanation:** All the possible subsequence sums that we can obtain are the following sorted in decreasing order:

- 6, 4, 4, 2, <ins>2</ins>, 0, 0, -2.

The 5-Sum of the array is 2. 

**Example 2:**

**Input:** nums = [1,-2,3,4,-10,12], k = 16

**Output:** 10

**Explanation:** The 16-Sum of the array is 10. 

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= min(2000, 2<sup>n</sup>)</code>

## Solution

```kotlin
import java.util.PriorityQueue

@Suppress("NAME_SHADOWING")
class Solution {
    fun kSum(nums: IntArray, k: Int): Long {
        var k = k
        var sum = 0L
        for (i in nums.indices) {
            if (nums[i] > 0) {
                sum += nums[i].toLong()
            } else {
                nums[i] = -nums[i]
            }
        }
        nums.sort()
        val pq = PriorityQueue { a: Pair<Long, Int>, b: Pair<Long, Int> ->
            b.key.compareTo(a.key)
        }
        pq.offer(Pair(sum, 0))
        while (k-- > 1) {
            val top = pq.poll()
            val s: Long = top.key
            val i: Int = top.value
            if (i < nums.size) {
                pq.offer(Pair(s - nums[i], i + 1))
                if (i > 0) {
                    pq.offer(Pair(s - nums[i] + nums[i - 1], i + 1))
                }
            }
        }
        return pq.peek().key
    }

    private class Pair<K, V>(var key: K, var value: V)
}
```