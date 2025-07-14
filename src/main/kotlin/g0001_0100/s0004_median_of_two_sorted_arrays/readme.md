[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4\. Median of Two Sorted Arrays

Hard

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

**Input:** nums1 = [1,3], nums2 = [2]

**Output:** 2.00000

**Explanation:** merged array = [1,2,3] and median is 2. 

**Example 2:**

**Input:** nums1 = [1,2], nums2 = [3,4]

**Output:** 2.50000

**Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5. 

**Example 3:**

**Input:** nums1 = [0,0], nums2 = [0,0]

**Output:** 0.00000 

**Example 4:**

**Input:** nums1 = [], nums2 = [1]

**Output:** 1.00000 

**Example 5:**

**Input:** nums1 = [2], nums2 = []

**Output:** 2.00000 

**Constraints:**

*   `nums1.length == m`
*   `nums2.length == n`
*   `0 <= m <= 1000`
*   `0 <= n <= 1000`
*   `1 <= m + n <= 2000`
*   <code>-10<sup>6</sup> <= nums1[i], nums2[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun findMedianSortedArrays(nums1: IntArray, nums2: IntArray): Double {
        if (nums2.size < nums1.size) {
            return findMedianSortedArrays(nums2, nums1)
        }
        val n1 = nums1.size
        val n2 = nums2.size
        var low = 0
        var high = n1
        while (low <= high) {
            val cut1 = (low + high) / 2
            val cut2 = ((n1 + n2 + 1) / 2) - cut1
            val l1 = if (cut1 == 0) Int.MIN_VALUE else nums1[cut1 - 1]
            val l2 = if (cut2 == 0) Int.MIN_VALUE else nums2[cut2 - 1]
            val r1 = if (cut1 == n1) Int.MAX_VALUE else nums1[cut1]
            val r2 = if (cut2 == n2) Int.MAX_VALUE else nums2[cut2]
            if (l1 <= r2 && l2 <= r1) {
                return if ((n1 + n2) % 2 == 0) {
                    (max(l1, l2).toDouble() + min(r1, r2).toDouble()) / 2.0
                } else {
                    max(l1, l2).toDouble()
                }
            } else if (l1 > r2) {
                high = cut1 - 1
            } else {
                low = cut1 + 1
            }
        }
        return 0.0
    }
}
```