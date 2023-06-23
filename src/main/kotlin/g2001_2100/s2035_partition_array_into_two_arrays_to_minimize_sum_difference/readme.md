[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2035\. Partition Array Into Two Arrays to Minimize Sum Difference

Hard

You are given an integer array `nums` of `2 * n` integers. You need to partition `nums` into **two** arrays of length `n` to **minimize the absolute difference** of the **sums** of the arrays. To partition `nums`, put each element of `nums` into **one** of the two arrays.

Return _the **minimum** possible absolute difference_.

**Example 1:**

![example-1](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

**Input:** nums = [3,9,7,3]

**Output:** 2

**Explanation:** One optimal partition is: [3,9] and [7,3]. The absolute difference between the sums of the arrays is abs((3 + 9) - (7 + 3)) = 2.

**Example 2:**

**Input:** nums = [-36,36]

**Output:** 72

**Explanation:** One optimal partition is: [-36] and [36]. The absolute difference between the sums of the arrays is abs((-36) - (36)) = 72.

**Example 3:**

![example-3](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

**Input:** nums = [2,-1,0,4,-2,-9]

**Output:** 0

**Explanation:** One optimal partition is: [2,4,-9] and [-1,0,-2]. The absolute difference between the sums of the arrays is abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0.

**Constraints:**

*   `1 <= n <= 15`
*   `nums.length == 2 * n`
*   <code>-10<sup>7</sup> <= nums[i] <= 10<sup>7</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumDifference(nums: IntArray): Int {
        if (nums.isEmpty()) {
            return -1
        }
        val n = nums.size / 2
        var sum = 0
        val arr1: MutableList<MutableList<Int>> = ArrayList()
        val arr2: MutableList<MutableList<Int>> = ArrayList()
        for (i in 0..n) {
            arr1.add(ArrayList())
            arr2.add(ArrayList())
            if (i < n) {
                sum += nums[i]
                sum += nums[i + n]
            }
        }
        for (state in 0 until (1 shl n)) {
            var sum1 = 0
            var sum2 = 0
            for (i in 0 until n) {
                if (state and (1 shl i) == 0) {
                    continue
                }
                val a1 = nums[i]
                val a2 = nums[i + n]
                sum1 += a1
                sum2 += a2
            }
            val numOfEleInSet = Integer.bitCount(state)
            arr1[numOfEleInSet].add(sum1)
            arr2[numOfEleInSet].add(sum2)
        }
        for (i in 0..n) {
            arr2[i].sort()
        }
        var min = Int.MAX_VALUE
        for (i in 0..n) {
            val sums1: List<Int> = arr1[i]
            val sums2: List<Int> = arr2[n - i]
            for (s1 in sums1) {
                var idx = sums2.binarySearch(sum / 2 - s1)
                if (idx < 0) {
                    idx = -(idx + 1)
                }
                if (idx < sums1.size) {
                    min = Math.min(
                        min,
                        Math.abs(sum - s1 - sums2[idx] - (sums2[idx] + s1))
                    )
                }
                if (idx - 1 >= 0) {
                    min = Math.min(
                        min,
                        Math.abs(
                            sum - s1 - sums2[idx - 1] -
                                (sums2[idx - 1] + s1)
                        )
                    )
                }
            }
        }
        return min
    }
}
```