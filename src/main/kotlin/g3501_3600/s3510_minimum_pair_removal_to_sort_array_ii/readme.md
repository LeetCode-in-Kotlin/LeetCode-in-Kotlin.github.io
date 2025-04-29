[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3510\. Minimum Pair Removal to Sort Array II

Hard

Given an array `nums`, you can perform the following operation any number of times:

*   Select the **adjacent** pair with the **minimum** sum in `nums`. If multiple such pairs exist, choose the leftmost one.
*   Replace the pair with their sum.

Return the **minimum number of operations** needed to make the array **non-decreasing**.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous element (if it exists).

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** 2

**Explanation:**

*   The pair `(3,1)` has the minimum sum of 4. After replacement, `nums = [5,2,4]`.
*   The pair `(2,4)` has the minimum sum of 6. After replacement, `nums = [5,6]`.

The array `nums` became non-decreasing in two operations.

**Example 2:**

**Input:** nums = [1,2,2]

**Output:** 0

**Explanation:**

The array `nums` is already sorted.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.ceil
import kotlin.math.ln
import kotlin.math.min
import kotlin.math.pow

class Solution {
    fun minimumPairRemoval(nums: IntArray): Int {
        if (nums.size == 1) {
            return 0
        }
        val size = 2.0.pow(ceil(ln(nums.size - 1.0) / ln(2.0))).toInt()
        val segment = LongArray(size * 2 - 1)
        segment.fill(Long.Companion.MAX_VALUE)
        val lefts = IntArray(size * 2 - 1)
        val rights = IntArray(size * 2 - 1)
        val sums = LongArray(nums.size)
        sums.fill(Long.Companion.MAX_VALUE / 2)
        val arrIdxToSegIdx: Array<IntArray> = Array(nums.size) { IntArray(0) }
        sums[0] = nums[0].toLong()
        var count = 0
        arrIdxToSegIdx[0] = intArrayOf(-1, size - 1)
        for (i in 1..<nums.size) {
            if (nums[i] < nums[i - 1]) {
                count++
            }
            lefts[size + i - 2] = i - 1
            rights[size + i - 2] = i
            segment[size + i - 2] = nums[i - 1] + nums[i].toLong()
            arrIdxToSegIdx[i] = intArrayOf(size + i - 2, size + i - 1)
            sums[i] = nums[i].toLong()
        }
        arrIdxToSegIdx[nums.size - 1][1] = -1
        for (i in size - 2 downTo 0) {
            val l = 2 * i + 1
            val r = 2 * i + 2
            segment[i] = min(segment[l], segment[r])
        }
        return getRes(count, segment, lefts, rights, sums, arrIdxToSegIdx)
    }

    private fun getRes(
        count: Int,
        segment: LongArray,
        lefts: IntArray,
        rights: IntArray,
        sums: LongArray,
        arrIdxToSegIdx: Array<IntArray>,
    ): Int {
        var count = count
        var res = 0
        while (count > 0) {
            var segIdx = 0
            while (2 * segIdx + 1 < segment.size) {
                val l = 2 * segIdx + 1
                val r = 2 * segIdx + 2
                segIdx = if (segment[l] <= segment[r]) {
                    l
                } else {
                    r
                }
            }
            val arrIdxL = lefts[segIdx]
            val arrIdxR = rights[segIdx]
            val numL = sums[arrIdxL]
            val numR = sums[arrIdxR]
            if (numL > numR) {
                count--
            }
            sums[arrIdxL] = sums[arrIdxL] + sums[arrIdxR]
            val newSum = sums[arrIdxL]
            val leftPointer = arrIdxToSegIdx[arrIdxL]
            val rightPointer = arrIdxToSegIdx[arrIdxR]
            val prvSegIdx = leftPointer[0]
            val nextSegIdx = rightPointer[1]
            leftPointer[1] = nextSegIdx
            if (prvSegIdx != -1) {
                val l = lefts[prvSegIdx]
                if (sums[l] > numL && sums[l] <= newSum) {
                    count--
                } else if (sums[l] <= numL && sums[l] > newSum) {
                    count++
                }
                modify(segment, prvSegIdx, sums[l] + newSum)
            }
            if (nextSegIdx != -1) {
                val r = rights[nextSegIdx]
                if (numR > sums[r] && newSum <= sums[r]) {
                    count--
                } else if (numR <= sums[r] && newSum > sums[r]) {
                    count++
                }
                modify(segment, nextSegIdx, newSum + sums[r])
                lefts[nextSegIdx] = arrIdxL
            }
            modify(segment, segIdx, Long.Companion.MAX_VALUE)
            res++
        }
        return res
    }

    private fun modify(segment: LongArray, idx: Int, num: Long) {
        var idx = idx
        if (segment[idx] == num) {
            return
        }
        segment[idx] = num
        while (idx != 0) {
            idx = (idx - 1) / 2
            val l = 2 * idx + 1
            val r = 2 * idx + 2
            segment[idx] = min(segment[l], segment[r])
        }
    }
}
```