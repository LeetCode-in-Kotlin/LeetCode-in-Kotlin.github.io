[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2541\. Minimum Operations to Make Array Equal II

Medium

You are given two integer arrays `nums1` and `nums2` of equal length `n` and an integer `k`. You can perform the following operation on `nums1`:

*   Choose two indexes `i` and `j` and increment `nums1[i]` by `k` and decrement `nums1[j]` by `k`. In other words, `nums1[i] = nums1[i] + k` and `nums1[j] = nums1[j] - k`.

`nums1` is said to be **equal** to `nums2` if for all indices `i` such that `0 <= i < n`, `nums1[i] == nums2[i]`.

Return _the **minimum** number of operations required to make_ `nums1` _equal to_ `nums2`. If it is impossible to make them equal, return `-1`.

**Example 1:**

**Input:** nums1 = [4,3,1,4], nums2 = [1,3,7,1], k = 3

**Output:** 2

**Explanation:** In 2 operations, we can transform nums1 to nums2. 

1<sup>st</sup> operation: i = 2, j = 0. After applying the operation, nums1 = [1,3,4,4]. 

2<sup>nd</sup> operation: i = 2, j = 3. After applying the operation, nums1 = [1,3,7,1]. One can prove that it is impossible to make arrays equal in fewer operations.

**Example 2:**

**Input:** nums1 = [3,8,5,2], nums2 = [2,4,1,6], k = 1

**Output:** -1

**Explanation:** It can be proved that it is impossible to make the two arrays equal.

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums1[i], nums2[j] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun minOperations(nums1: IntArray, nums2: IntArray, k: Int): Long {
        val n = nums1.size
        var pcnt: Long = 0
        var ncnt: Long = 0
        if (k == 0) {
            return if (nums1.contentEquals(nums2)) {
                0
            } else {
                -1
            }
        }
        for (i in 0 until n) {
            val tp = nums1[i] - nums2[i]
            if (tp % k != 0) {
                return -1
            }
            if (tp > 0) {
                pcnt += tp.toLong()
            } else if (tp < 0) {
                ncnt += tp.toLong()
            }
        }
        return if (pcnt + ncnt != 0L) {
            -1
        } else pcnt / k
    }
}
```