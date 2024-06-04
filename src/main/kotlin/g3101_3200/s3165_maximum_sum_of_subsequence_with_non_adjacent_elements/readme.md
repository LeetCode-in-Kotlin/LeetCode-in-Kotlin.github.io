[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3165\. Maximum Sum of Subsequence With Non-adjacent Elements

Hard

You are given an array `nums` consisting of integers. You are also given a 2D array `queries`, where <code>queries[i] = [pos<sub>i</sub>, x<sub>i</sub>]</code>.

For query `i`, we first set <code>nums[pos<sub>i</sub>]</code> equal to <code>x<sub>i</sub></code>, then we calculate the answer to query `i` which is the **maximum** sum of a subsequence of `nums` where **no two adjacent elements are selected**.

Return the _sum_ of the answers to all queries.

Since the final answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [3,5,9], queries = \[\[1,-2],[0,-3]]

**Output:** 21

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is `3 + 9 = 12`.   
 After the 2<sup>nd</sup> query, `nums = [-3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is 9.

**Example 2:**

**Input:** nums = [0,-1], queries = \[\[0,-5]]

**Output:** 0

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [-5,-1]` and the maximum sum of a subsequence with non-adjacent elements is 0 (choosing an empty subsequence).

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>queries[i] == [pos<sub>i</sub>, x<sub>i</sub>]</code>
*   <code>0 <= pos<sub>i</sub> <= nums.length - 1</code>
*   <code>-10<sup>5</sup> <= x<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.stream.Stream
import kotlin.math.max

class Solution {
    fun maximumSumSubsequence(nums: IntArray, queries: Array<IntArray>): Int {
        var ans = 0
        val segTree = SegTree(nums)
        for (q in queries) {
            val idx = q[0]
            val `val` = q[1]
            segTree.update(idx, `val`)
            ans = (ans + segTree.max!!) % MOD
        }
        return ans
    }

    internal class SegTree(private val nums: IntArray) {
        private class Record {
            var takeFirstTakeLast: Int = 0
            var takeFirstSkipLast: Int = 0
            var skipFirstSkipLast: Int = 0
            var skipFirstTakeLast: Int = 0

            val max: Int
                get() = Stream.of(
                    this.takeFirstSkipLast,
                    this.takeFirstTakeLast,
                    this.skipFirstSkipLast,
                    this.skipFirstTakeLast
                )
                    .max { x: Int?, y: Int? -> x!!.compareTo(y!!) }
                    .orElse(null)

            fun skipLast(): Int? {
                return Stream.of(this.takeFirstSkipLast, this.skipFirstSkipLast)
                    .max { x: Int?, y: Int? -> x!!.compareTo(y!!) }
                    .orElse(null)
            }

            fun takeLast(): Int? {
                return Stream.of(this.skipFirstTakeLast, this.takeFirstTakeLast)
                    .max { x: Int?, y: Int? -> x!!.compareTo(y!!) }
                    .orElse(null)
            }
        }

        private val seg = arrayOfNulls<Record>(4 * nums.size)

        init {
            for (i in 0 until 4 * nums.size) {
                seg[i] = Record()
            }
            build(0, nums.size - 1, 0)
        }

        private fun build(i: Int, j: Int, k: Int) {
            if (i == j) {
                seg[k]!!.takeFirstTakeLast = nums[i]
                return
            }
            val mid = (i + j) shr 1
            build(i, mid, 2 * k + 1)
            build(mid + 1, j, 2 * k + 2)
            merge(k)
        }

        // merge [2*k+1, 2*k+2] into k
        private fun merge(k: Int) {
            seg[k]!!.takeFirstSkipLast = max(
                (seg[2 * k + 1]!!.takeFirstSkipLast + seg[2 * k + 2]!!.skipLast()!!),
                (seg[2 * k + 1]!!.takeFirstTakeLast + seg[2 * k + 2]!!.skipFirstSkipLast)
            )

            seg[k]!!.takeFirstTakeLast = max(
                (seg[2 * k + 1]!!.takeFirstSkipLast + seg[2 * k + 2]!!.takeLast()!!),
                (seg[2 * k + 1]!!.takeFirstTakeLast + seg[2 * k + 2]!!.skipFirstTakeLast)
            )

            seg[k]!!.skipFirstTakeLast = max(
                (seg[2 * k + 1]!!.skipFirstSkipLast + seg[2 * k + 2]!!.takeLast()!!),
                (seg[2 * k + 1]!!.skipFirstTakeLast + seg[2 * k + 2]!!.skipFirstTakeLast)
            )

            seg[k]!!.skipFirstSkipLast = max(
                (seg[2 * k + 1]!!.skipFirstSkipLast + seg[2 * k + 2]!!.skipLast()!!),
                (seg[2 * k + 1]!!.skipFirstTakeLast + seg[2 * k + 2]!!.skipFirstSkipLast)
            )
        }

        // child -> parent
        fun update(idx: Int, `val`: Int) {
            val i = 0
            val j = nums.size - 1
            val k = 0
            update(idx, `val`, k, i, j)
        }

        private fun update(idx: Int, `val`: Int, k: Int, i: Int, j: Int) {
            if (i == j) {
                seg[k]!!.takeFirstTakeLast = `val`
                return
            }
            val mid = (i + j) shr 1
            if (idx <= mid) {
                update(idx, `val`, 2 * k + 1, i, mid)
            } else {
                update(idx, `val`, 2 * k + 2, mid + 1, j)
            }
            merge(k)
        }

        val max: Int?
            get() = seg[0]?.max
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```