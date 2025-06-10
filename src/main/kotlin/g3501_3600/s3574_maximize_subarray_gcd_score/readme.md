[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3574\. Maximize Subarray GCD Score

Hard

You are given an array of positive integers `nums` and an integer `k`.

You may perform at most `k` operations. In each operation, you can choose one element in the array and **double** its value. Each element can be doubled **at most** once.

The **score** of a contiguous **subarray** is defined as the **product** of its length and the _greatest common divisor (GCD)_ of all its elements.

Your task is to return the **maximum** **score** that can be achieved by selecting a contiguous subarray from the modified array.

**Note:**

*   The **greatest common divisor (GCD)** of an array is the largest integer that evenly divides all the array elements.

**Example 1:**

**Input:** nums = [2,4], k = 1

**Output:** 8

**Explanation:**

*   Double `nums[0]` to 4 using one operation. The modified array becomes `[4, 4]`.
*   The GCD of the subarray `[4, 4]` is 4, and the length is 2.
*   Thus, the maximum possible score is `2 × 4 = 8`.

**Example 2:**

**Input:** nums = [3,5,7], k = 2

**Output:** 14

**Explanation:**

*   Double `nums[2]` to 14 using one operation. The modified array becomes `[3, 5, 14]`.
*   The GCD of the subarray `[14]` is 14, and the length is 1.
*   Thus, the maximum possible score is `1 × 14 = 14`.

**Example 3:**

**Input:** nums = [5,5,5], k = 1

**Output:** 15

**Explanation:**

*   The subarray `[5, 5, 5]` has a GCD of 5, and its length is 3.
*   Since doubling any element doesn't improve the score, the maximum score is `3 × 5 = 15`.

**Constraints:**

*   `1 <= n == nums.length <= 1500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxGCDScore(nums: IntArray, k: Int): Long {
        var mx = 0
        for (x in nums) {
            mx = max(mx, x)
        }
        val width = 32 - Integer.numberOfLeadingZeros(mx)
        val lowBitPos: Array<MutableList<Int>> = Array<MutableList<Int>>(width) { _ -> ArrayList<Int>() }
        val intervals = Array<IntArray>(width + 1) { IntArray(3) }
        var size = 0
        var ans: Long = 0
        for (i in nums.indices) {
            val x = nums[i]
            val tz = Integer.numberOfTrailingZeros(x)
            lowBitPos[tz].add(i)
            for (j in 0..<size) {
                intervals[j][0] = gcd(intervals[j][0], x)
            }
            intervals[size][0] = x
            intervals[size][1] = i - 1
            intervals[size][2] = i
            size++
            var idx = 1
            for (j in 1..<size) {
                if (intervals[j][0] != intervals[j - 1][0]) {
                    intervals[idx][0] = intervals[j][0]
                    intervals[idx][1] = intervals[j][1]
                    intervals[idx][2] = intervals[j][2]
                    idx++
                } else {
                    intervals[idx - 1][2] = intervals[j][2]
                }
            }
            size = idx
            for (j in 0..<size) {
                val g = intervals[j][0]
                val l = intervals[j][1]
                val r = intervals[j][2]
                ans = max(ans, g.toLong() * (i - l))
                val pos = lowBitPos[Integer.numberOfTrailingZeros(g)]
                val minL = if (pos.size > k) max(l, pos[pos.size - k - 1]) else l
                if (minL < r) {
                    ans = max(ans, g.toLong() * 2 * (i - minL))
                }
            }
        }
        return ans
    }

    private fun gcd(a: Int, b: Int): Int {
        var a = a
        var b = b
        while (a != 0) {
            val tmp = a
            a = b % a
            b = tmp
        }
        return b
    }
}
```