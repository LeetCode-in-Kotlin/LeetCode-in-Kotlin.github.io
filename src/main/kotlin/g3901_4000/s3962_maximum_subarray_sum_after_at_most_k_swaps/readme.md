[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3962\. Maximum Subarray Sum After at Most K Swaps

Hard

You are given an integer array `nums` and an integer `k`.

You are allowed to perform **at most** `k` swap operations on the array.

In one swap operation, you may choose any two indices `i` and `j` and swap `nums[i]` and `nums[j]`.

Return an integer denoting the **maximum possible non-empty subarrays sum** after performing the swaps.

**Example 1:**

**Input:** nums = [1,-1,0,2], k = 1

**Output:** 3

**Explanation:**

*   We can swap on indices 1 and 3, resulting in the array `[1, 2, 0, -1]`.
*   The subarray `[1, 2]` has a sum of 3, which is the maximum possible subarray sum after at most `k = 1` swap.

**Example 2:**

**Input:** nums = [4,3,2,4], k = 2

**Output:** 13

**Explanation:**

The maximum possible subarray sum after at most `k = 2` swaps is the sum of the entire array, which is 13.

**Example 3:**

**Input:** nums = [-1,-2], k = 0

**Output:** \-1

**Explanation:**

*   `k = 0` swaps are allowed.
*   The possible subarrays are `[-1]`, `[-2]`, and `[-1, -2]`, with sums -1, -2, and -3 respectively.
*   Among these sums, the maximum is -1.

**Constraints:**

*   `1 <= nums.length <= 1500`
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `0 <= k <= nums.length`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxSum(nums: IntArray, k: Int): Long {
        if (k == 0) {
            return maxSubarraySum(nums)
        }
        val info = analyzePositiveValues(nums)
        if (info.count == 0) {
            return info.maxElement
        }
        val maxPositiveWindow = findMaxPositiveWindow(nums, info.count)
        if (k >= info.count - maxPositiveWindow) {
            return info.sum
        }
        val values = getUniqueSortedValues(nums)
        val map = mapValues(nums, values)
        val totalCounts = countMappedValues(map, values.size)
        return findBestSum(nums, k, values, map, totalCounts)
    }

    private fun maxSubarraySum(nums: IntArray): Long {
        var max = nums[0].toLong()
        var cur = nums[0].toLong()
        for (i in 1..<nums.size) {
            cur = max(nums[i].toLong(), cur + nums[i])
            max = max(max, cur)
        }
        return max
    }

    private fun analyzePositiveValues(nums: IntArray): PositiveInfo {
        var count = 0
        var sum: Long = 0
        var maxElement = Long.MIN_VALUE
        for (x in nums) {
            if (x > 0) {
                sum += x.toLong()
                count++
            }
            maxElement = max(maxElement, x.toLong())
        }
        return PositiveInfo(count, sum, maxElement)
    }

    private fun findMaxPositiveWindow(nums: IntArray, windowSize: Int): Int {
        var current = 0
        var max = 0
        for (i in nums.indices) {
            if (nums[i] > 0) {
                current++
            }
            if (i >= windowSize && nums[i - windowSize] > 0) {
                current--
            }
            if (i >= windowSize - 1) {
                max = max(max, current)
            }
        }
        return max
    }

    private fun getUniqueSortedValues(nums: IntArray): IntArray {
        val sorted = nums.clone()
        sorted.sort()
        var size = 0
        for (x in sorted) {
            if (size == 0 || sorted[size - 1] != x) {
                sorted[size++] = x
            }
        }
        return sorted.copyOf(size)
    }

    private fun mapValues(nums: IntArray, values: IntArray): IntArray {
        val map = IntArray(nums.size)
        for (i in nums.indices) {
            map[i] = values.binarySearch(nums[i])
        }
        return map
    }

    private fun countMappedValues(map: IntArray, size: Int): IntArray {
        val counts = IntArray(size)
        for (index in map) {
            counts[index]++
        }
        return counts
    }

    private fun findBestSum(nums: IntArray, k: Int, values: IntArray, map: IntArray, totalCounts: IntArray): Long {
        var best = Long.MIN_VALUE
        val included = IntArray(values.size)
        for (i in nums.indices) {
            included.fill(0)
            val context = SwapContext(values, included, totalCounts)
            best = evaluateStartingPosition(nums, i, k, map, context, best)
        }
        return best
    }

    private fun evaluateStartingPosition(
        nums: IntArray,
        start: Int,
        k: Int,
        map: IntArray,
        context: SwapContext,
        best: Long,
    ): Long {
        var best = best
        var sum: Long = 0
        var minIndex = context.values.size
        var maxOutside = context.values.size - 1
        for (j in start..<nums.size) {
            val index = map[j]
            context.included[index]++
            sum += nums[j].toLong()
            minIndex = min(minIndex, index)
            maxOutside = findMaxOutside(maxOutside, context)
            if (hasNoUsefulSwap(minIndex, maxOutside, context.values)) {
                best = max(best, sum)
            } else if (sum + k.toLong() * (context.values[maxOutside] - context.values[minIndex])
                > best
            ) {
                val candidate = optimizeSwaps(k, sum, minIndex, maxOutside, context)
                best = max(best, candidate)
            }
        }
        return best
    }

    private fun findMaxOutside(maxOutside: Int, context: SwapContext): Int {
        var maxOutside = maxOutside
        while (maxOutside >= 0 && context.totalCounts[maxOutside] == context.included[maxOutside]) {
            maxOutside--
        }
        return maxOutside
    }

    private fun hasNoUsefulSwap(minIndex: Int, maxOutside: Int, values: IntArray): Boolean =
        maxOutside < 0 || values[minIndex] >= values[maxOutside]

    private fun optimizeSwaps(k: Int, sum: Long, minIndex: Int, maxOutside: Int, context: SwapContext): Long {
        val values = context.values
        val included = context.included
        val totalCounts = context.totalCounts
        var result = sum
        var swaps = 0
        var left = minIndex
        var right = maxOutside
        var leftCount = included[left]
        var rightCount = totalCounts[right] - included[right]

        while (swaps < k && left < right) {
            if (leftCount == 0) {
                left++
                if (left < right) {
                    leftCount = included[left]
                }
            } else if (rightCount == 0) {
                right--
                if (left < right) {
                    rightCount = totalCounts[right] - included[right]
                }
            } else if (values[left] >= values[right]) {
                break
            } else {
                val take = min(k - swaps, min(leftCount, rightCount))
                result += take.toLong() * (values[right] - values[left])
                swaps += take
                leftCount -= take
                rightCount -= take
            }
        }
        return result
    }

    internal class SwapContext(val values: IntArray, val included: IntArray, val totalCounts: IntArray) {
        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other !is SwapContext) {
                return false
            }
            val other = other
            return values.contentEquals(other.values) && included.contentEquals(other.included) &&
                totalCounts.contentEquals(
                    other.totalCounts,
                )
        }

        override fun hashCode(): Int {
            var result = values.contentHashCode()
            result = 31 * result + included.contentHashCode()
            result = 31 * result + totalCounts.contentHashCode()
            return result
        }

        override fun toString(): String = (
            "SwapContext[values=" +
                values.contentToString() + ", included=" +
                included.contentToString() + ", totalCounts=" +
                totalCounts.contentToString() + "]"
            )
    }

    private class PositiveInfo(val count: Int, val sum: Long, val maxElement: Long)
}
```