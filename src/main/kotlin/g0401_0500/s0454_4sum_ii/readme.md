[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 454\. 4Sum II

Medium

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

*   `0 <= i, j, k, l < n`
*   `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

**Example 1:**

**Input:** nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]

**Output:** 2

**Explanation:** The two tuples are: 

1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0

2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0

**Example 2:**

**Input:** nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]

**Output:** 1

**Constraints:**

*   `n == nums1.length`
*   `n == nums2.length`
*   `n == nums3.length`
*   `n == nums4.length`
*   `1 <= n <= 200`
*   <code>-2<sup>28</sup> <= nums1[i], nums2[i], nums3[i], nums4[i] <= 2<sup>28</sup></code>

## Solution

```kotlin
class Solution {
    fun fourSumCount(nums1: IntArray, nums2: IntArray, nums3: IntArray, nums4: IntArray): Int {
        var count = 0
        val map: MutableMap<Int, Int> = HashMap()
        for (k in nums3) {
            for (i in nums4) {
                val sum = k + i
                map[sum] = map.getOrDefault(sum, 0) + 1
            }
        }
        for (k in nums1) {
            for (i in nums2) {
                val m = -(k + i)
                count += map.getOrDefault(m, 0)
            }
        }
        return count
    }
}
```