[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 870\. Advantage Shuffle

Medium

You are given two integer arrays `nums1` and `nums2` both of the same length. The **advantage** of `nums1` with respect to `nums2` is the number of indices `i` for which `nums1[i] > nums2[i]`.

Return _any permutation of_ `nums1` _that maximizes its **advantage** with respect to_ `nums2`.

**Example 1:**

**Input:** nums1 = [2,7,11,15], nums2 = [1,10,4,11]

**Output:** [2,11,7,15]

**Example 2:**

**Input:** nums1 = [12,24,8,32], nums2 = [13,25,32,11]

**Output:** [24,32,8,12]

**Constraints:**

*   <code>1 <= nums1.length <= 10<sup>5</sup></code>
*   `nums2.length == nums1.length`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun advantageCount(nums1: IntArray, nums2: IntArray): IntArray {
        val n = nums1.size
        nums1.sort()
        val maxpq = PriorityQueue { pair1: IntArray, pair2: IntArray ->
            pair2[1] - pair1[1]
        }
        for (i in 0 until n) {
            maxpq.offer(intArrayOf(i, nums2[i]))
        }
        var left = 0
        var right = n - 1
        val res = IntArray(n)
        while (!maxpq.isEmpty()) {
            val pair = maxpq.poll()
            val i = pair[0]
            val `val` = pair[1]
            if (nums1[right] > `val`) {
                res[i] = nums1[right]
                right--
            } else {
                res[i] = nums1[left]
                left++
            }
        }
        return res
    }
}
```