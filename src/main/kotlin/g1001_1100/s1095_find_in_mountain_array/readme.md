[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1095\. Find in Mountain Array

Hard

_(This problem is an **interactive problem**.)_

You may recall that an array `arr` is a **mountain array** if and only if:

*   `arr.length >= 3`
*   There exists some `i` with `0 < i < arr.length - 1` such that:
    *   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    *   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `mountainArr`, return the **minimum** `index` such that `mountainArr.get(index) == target`. If such an `index` does not exist, return `-1`.

**You cannot access the mountain array directly.** You may only access the array using a `MountainArray` interface:

*   `MountainArray.get(k)` returns the element of the array at index `k` (0-indexed).
*   `MountainArray.length()` returns the length of the array.

Submissions making more than `100` calls to `MountainArray.get` will be judged _Wrong Answer_. Also, any solutions that attempt to circumvent the judge will result in disqualification.

**Example 1:**

**Input:** array = [1,2,3,4,5,3,1], target = 3

**Output:** 2

**Explanation:** 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.

**Example 2:**

**Input:** array = [0,1,2,4,2,1], target = 3

**Output:** -1

**Explanation:** 3 does not exist in `the array,` so we return -1.

**Constraints:**

*   <code>3 <= mountain_arr.length() <= 10<sup>4</sup></code>
*   <code>0 <= target <= 10<sup>9</sup></code>
*   <code>0 <= mountain_arr.get(index) <= 10<sup>9</sup></code>

## Solution

```kotlin
/*
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *     fun get(index: Int): Int {}
 *     fun length(): Int {}
 * }
 */

class Solution {
    fun findInMountainArray(target: Int, mountainArr: MountainArray): Int {
        val peakIndex = findPeak(mountainArr)
        if (target == mountainArr.get(peakIndex)) {
            return peakIndex
        }
        val leftResult = findInPeakLeft(target, peakIndex, mountainArr)
        return if (leftResult != -1) {
            leftResult
        } else {
            findInPeakRight(target, peakIndex, mountainArr)
        }
    }

    private fun findPeak(mountainArray: MountainArray): Int {
        val len: Int = mountainArray.length()
        var left = 0
        var right = len - 1
        while (left < right) {
            val mid = left + (right - left) / 2
            if (mountainArray.get(mid) < mountainArray.get(mid + 1)) {
                left = mid + 1
            } else {
                right = mid
            }
        }
        return left
    }

    private fun findInPeakLeft(target: Int, peakIndex: Int, mountainArray: MountainArray): Int {
        var leftIndex = 0
        var rightIndex = peakIndex - 1
        while (leftIndex < rightIndex) {
            val midIndex = leftIndex + (rightIndex - leftIndex) / 2
            if (target > mountainArray.get(midIndex)) {
                leftIndex = midIndex + 1
            } else {
                rightIndex = midIndex
            }
        }
        return if (target == mountainArray.get(leftIndex)) leftIndex else -1
    }

    private fun findInPeakRight(target: Int, peakIndex: Int, mountainArray: MountainArray): Int {
        var leftIndex = peakIndex + 1
        var rightIndex: Int = mountainArray.length() - 1
        while (leftIndex < rightIndex) {
            val midIndex = leftIndex + (rightIndex - leftIndex) / 2
            if (target < mountainArray.get(midIndex)) {
                leftIndex = midIndex + 1
            } else {
                rightIndex = midIndex
            }
        }
        return if (target == mountainArray.get(leftIndex)) leftIndex else -1
    }
}
```