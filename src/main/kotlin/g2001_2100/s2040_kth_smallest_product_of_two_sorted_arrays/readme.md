[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2040\. Kth Smallest Product of Two Sorted Arrays

Hard

Given two **sorted 0-indexed** integer arrays `nums1` and `nums2` as well as an integer `k`, return _the_ <code>k<sup>th</sup></code> _(**1-based**) smallest product of_ `nums1[i] * nums2[j]` _where_ `0 <= i < nums1.length` _and_ `0 <= j < nums2.length`.

**Example 1:**

**Input:** nums1 = [2,5], nums2 = [3,4], k = 2

**Output:** 8

**Explanation:** The 2 smallest products are: 

- nums1[0] \* nums2[0] = 2 \* 3 = 6 

- nums1[0] \* nums2[1] = 2 \* 4 = 8 
  
The 2<sup>nd</sup> smallest product is 8.

**Example 2:**

**Input:** nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6

**Output:** 0

**Explanation:** The 6 smallest products are: 

- nums1[0] \* nums2[1] = (-4) \* 4 = -16 

- nums1[0] \* nums2[0] = (-4) \* 2 = -8 

- nums1[1] \* nums2[1] = (-2) \* 4 = -8 

- nums1[1] \* nums2[0] = (-2) \* 2 = -4 

- nums1[2] \* nums2[0] = 0 \* 2 = 0 

- nums1[2] \* nums2[1] = 0 \* 4 = 0 
  
The 6<sup>th</sup> smallest product is 0.

**Example 3:**

**Input:** nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3

**Output:** -6

**Explanation:** The 3 smallest products are: 

- nums1[0] \* nums2[4] = (-2) \* 5 = -10 

- nums1[0] \* nums2[3] = (-2) \* 4 = -8 

- nums1[4] \* nums2[0] = 2 \* (-3) = -6 
  
The 3<sup>rd</sup> smallest product is -6.

**Constraints:**

*   <code>1 <= nums1.length, nums2.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums1[i], nums2[j] <= 10<sup>5</sup></code>
*   `1 <= k <= nums1.length * nums2.length`
*   `nums1` and `nums2` are sorted.

## Solution

```kotlin
class Solution {
    fun kthSmallestProduct(nums1: IntArray, nums2: IntArray, k: Long): Long {
        val n = nums2.size
        var lo = -inf - 1
        var hi = inf + 1
        while (lo < hi) {
            val mid = lo + (hi - lo shr 1)
            var cnt: Long = 0
            for (i in nums1) {
                var l = 0
                var r = n - 1
                var p = 0
                if (0 <= i) {
                    while (l <= r) {
                        val c = l + (r - l shr 1)
                        val mul = i * nums2[c].toLong()
                        if (mul <= mid) {
                            p = c + 1
                            l = c + 1
                        } else {
                            r = c - 1
                        }
                    }
                } else {
                    while (l <= r) {
                        val c = l + (r - l shr 1)
                        val mul = i * nums2[c].toLong()
                        if (mul <= mid) {
                            p = n - c
                            r = c - 1
                        } else {
                            l = c + 1
                        }
                    }
                }
                cnt += p.toLong()
            }
            if (cnt >= k) {
                hi = mid
            } else {
                lo = mid + 1L
            }
        }
        return lo
    }

    companion object {
        var inf = 1e10.toLong()
    }
}
```