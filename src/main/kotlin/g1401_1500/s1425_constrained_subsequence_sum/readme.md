[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1425\. Constrained Subsequence Sum

Hard

Given an integer array `nums` and an integer `k`, return the maximum sum of a **non-empty** subsequence of that array such that for every two **consecutive** integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A _subsequence_ of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

**Example 1:**

**Input:** nums = [10,2,-10,5,20], k = 2

**Output:** 37

**Explanation:** The subsequence is [10, 2, 5, 20].

**Example 2:**

**Input:** nums = [-1,-2,-3], k = 1

**Output:** -1

**Explanation:** The subsequence must be non-empty, so we choose the largest number.

**Example 3:**

**Input:** nums = [10,-2,-10,-5,20], k = 2

**Output:** 23

**Explanation:** The subsequence is [10, -2, -5, 20].

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import java.util.LinkedList

class Solution {
    fun constrainedSubsetSum(nums: IntArray, k: Int): Int {
        val n = nums.size
        var res = Int.MIN_VALUE
        val mono = LinkedList<IntArray>()
        for (i in 0 until n) {
            var take = nums[i]
            while (mono.isNotEmpty() && i - mono.first[0] > k) {
                mono.removeFirst()
            }
            if (mono.isNotEmpty()) {
                val mx = Math.max(0, mono.first[1])
                take += mx
            }
            while (mono.isNotEmpty() && take > mono.last[1]) {
                mono.removeLast()
            }
            mono.add(intArrayOf(i, take))
            res = Math.max(res, take)
        }
        return res
    }
}
```