[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2321\. Maximum Score Of Spliced Array

Hard

You are given two **0-indexed** integer arrays `nums1` and `nums2`, both of length `n`.

You can choose two integers `left` and `right` where `0 <= left <= right < n` and **swap** the subarray `nums1[left...right]` with the subarray `nums2[left...right]`.

*   For example, if `nums1 = [1,2,3,4,5]` and `nums2 = [11,12,13,14,15]` and you choose `left = 1` and `right = 2`, `nums1` becomes `[1,**<ins>12,13</ins>**,4,5]` and `nums2` becomes `[11,**<ins>2,3</ins>**,14,15]`.

You may choose to apply the mentioned operation **once** or not do anything.

The **score** of the arrays is the **maximum** of `sum(nums1)` and `sum(nums2)`, where `sum(arr)` is the sum of all the elements in the array `arr`.

Return _the **maximum possible score**_.

A **subarray** is a contiguous sequence of elements within an array. `arr[left...right]` denotes the subarray that contains the elements of `nums` between indices `left` and `right` (**inclusive**).

**Example 1:**

**Input:** nums1 = [60,60,60], nums2 = [10,90,10]

**Output:** 210

**Explanation:** Choosing left = 1 and right = 1, we have nums1 = [60,<ins>**90**</ins>,60] and nums2 = [10,<ins>**60**</ins>,10].

The score is max(sum(nums1), sum(nums2)) = max(210, 80) = 210.

**Example 2:**

**Input:** nums1 = [20,40,20,70,30], nums2 = [50,20,50,40,20]

**Output:** 220

**Explanation:** Choosing left = 3, right = 4, we have nums1 = [20,40,20,<ins>**40,20**</ins>] and nums2 = [50,20,50,<ins>**70,30**</ins>].

The score is max(sum(nums1), sum(nums2)) = max(140, 220) = 220. 

**Example 3:**

**Input:** nums1 = [7,11,13], nums2 = [1,1,1]

**Output:** 31

**Explanation:** We choose not to swap any subarray.

The score is max(sum(nums1), sum(nums2)) = max(31, 3) = 31. 

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun maximumsSplicedArray(nums1: IntArray, nums2: IntArray): Int {
        var nums1 = nums1
        var nums2 = nums2
        var sum1 = 0
        var sum2 = 0
        val n = nums1.size
        for (num in nums1) {
            sum1 += num
        }
        for (num in nums2) {
            sum2 += num
        }
        if (sum2 > sum1) {
            val temp = sum2
            sum2 = sum1
            sum1 = temp
            val temparr = nums2
            nums2 = nums1
            nums1 = temparr
        }
        // now sum1>=sum2
        // maxEndingHere denotes the maximum sum subarray ending at current index(ie. element at
        // current index has to be included)
        // minEndingHere denotes the minimum sum subarray ending at current index
        var maxEndingHere: Int
        var minEndingHere: Int
        var maxSoFar: Int
        var minSoFar: Int
        var currEle: Int
        minSoFar = nums2[0] - nums1[0]
        maxSoFar = minSoFar
        minEndingHere = maxSoFar
        maxEndingHere = minEndingHere
        for (i in 1 until n) {
            currEle = nums2[i] - nums1[i]
            minEndingHere += currEle
            maxEndingHere += currEle
            if (maxEndingHere < currEle) {
                maxEndingHere = currEle
            }
            if (minEndingHere > currEle) {
                minEndingHere = currEle
            }
            maxSoFar = maxEndingHere.coerceAtLeast(maxSoFar)
            minSoFar = minEndingHere.coerceAtMost(minSoFar)
        }
        // return the maximum of the 2 possibilities dicussed
        // also keep care that maxSoFar>=0 and maxSoFar<=0
        return (sum1 + maxSoFar.coerceAtLeast(0)).coerceAtLeast(sum2 - 0.coerceAtMost(minSoFar))
    }
}
```