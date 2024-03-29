[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2333\. Minimum Sum of Squared Difference

Medium

You are given two positive **0-indexed** integer arrays `nums1` and `nums2`, both of length `n`.

The **sum of squared difference** of arrays `nums1` and `nums2` is defined as the **sum** of <code>(nums1[i] - nums2[i])<sup>2</sup></code> for each `0 <= i < n`.

You are also given two positive integers `k1` and `k2`. You can modify any of the elements of `nums1` by `+1` or `-1` at most `k1` times. Similarly, you can modify any of the elements of `nums2` by `+1` or `-1` at most `k2` times.

Return _the minimum **sum of squared difference** after modifying array_ `nums1` _at most_ `k1` _times and modifying array_ `nums2` _at most_ `k2` _times_.

**Note**: You are allowed to modify the array elements to become **negative** integers.

**Example 1:**

**Input:** nums1 = [1,2,3,4], nums2 = [2,10,20,19], k1 = 0, k2 = 0

**Output:** 579

**Explanation:** The elements in nums1 and nums2 cannot be modified because k1 = 0 and k2 = 0.

The sum of square difference will be: (1 - 2)<sup>2</sup> \+ (2 - 10)<sup>2</sup> \+ (3 - 20)<sup>2</sup> \+ (4 - 19)<sup>2</sup> = 579. 

**Example 2:**

**Input:** nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1

**Output:** 43

**Explanation:** One way to obtain the minimum sum of square difference is:

- Increase nums1[0] once.

- Increase nums2[2] once.

The minimum of the sum of square difference will be: (2 - 5)<sup>2</sup> \+ (4 - 8)<sup>2</sup> \+ (10 - 7)<sup>2</sup> \+ (12 - 9)<sup>2</sup> = 43.

Note that, there are other ways to obtain the minimum of the sum of square difference, but there is no way to obtain a sum smaller than 43.

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>5</sup></code>
*   <code>0 <= k1, k2 <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.pow

class Solution {
    fun minSumSquareDiff(nums1: IntArray, nums2: IntArray, k1: Int, k2: Int): Long {
        var minSumSquare: Long = 0
        val diffs = IntArray(100001)
        var totalDiff: Long = 0
        var kSum = k1.toLong() + k2
        var currentDiff: Int
        var maxDiff = 0
        for (i in nums1.indices) {
            // get current diff.
            currentDiff = abs(nums1[i] - nums2[i])
            // if current diff > 0, count/store it. If not,then ignore it.
            if (currentDiff > 0) {
                totalDiff += currentDiff.toLong()
                diffs[currentDiff]++
                maxDiff = maxDiff.coerceAtLeast(currentDiff)
            }
        }
        // if kSum (k1 + k2) < totalDifferences, it means we can make all numbers/differences 0s
        if (totalDiff <= kSum) {
            return 0
        }
        // starting from the back, from the highest difference, lower that group one by one to the
        // previous group.
        // we need to make all n diffs to n-1, then n-2, as long as kSum allows it.
        run {
            var i = maxDiff
            while (i > 0 && kSum > 0) {
                if (diffs[i] > 0) {
                    // if current group has more differences than the totalK, we can only move k of them
                    // to the lower level.
                    if (diffs[i] >= kSum) {
                        diffs[i] -= kSum.toInt()
                        diffs[i - 1] += kSum.toInt()
                        kSum = 0
                    } else {
                        // else, we can make this whole group one level lower.
                        diffs[i - 1] += diffs[i]
                        kSum -= diffs[i].toLong()
                        diffs[i] = 0
                    }
                }
                i--
            }
        }
        for (i in 0..maxDiff) {
            if (diffs[i] > 0) {
                minSumSquare += i.toDouble().pow(2.0).toLong() * diffs[i]
            }
        }
        return minSumSquare
    }
}
```