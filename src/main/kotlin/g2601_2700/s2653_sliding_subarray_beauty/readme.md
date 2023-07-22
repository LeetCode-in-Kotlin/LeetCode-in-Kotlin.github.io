[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2653\. Sliding Subarray Beauty

Medium

Given an integer array `nums` containing `n` integers, find the **beauty** of each subarray of size `k`.

The **beauty** of a subarray is the <code>x<sup>th</sup></code> **smallest integer** in the subarray if it is **negative**, or `0` if there are fewer than `x` negative integers.

Return _an integer array containing_ `n - k + 1` _integers, which denote the_ **beauty** _of the subarrays **in order** from the first index in the array._

*   A subarray is a contiguous **non-empty** sequence of elements within an array.


**Example 1:**

**Input:** nums = [1,-1,-3,-2,3], k = 3, x = 2

**Output:** [-1,-2,-2]

**Explanation:** There are 3 subarrays with size k = 3. 

The first subarray is `[1, -1, -3]` and the 2<sup>nd</sup> smallest negative integer is -1.

The second subarray is `[-1, -3, -2]` and the 2<sup>nd</sup> smallest negative integer is -2.

The third subarray is `[-3, -2, 3] `and the 2<sup>nd</sup> smallest negative integer is -2.

**Example 2:**

**Input:** nums = [-1,-2,-3,-4,-5], k = 2, x = 2

**Output:** [-1,-2,-3,-4]

**Explanation:** There are 4 subarrays with size k = 2. 

For `[-1, -2]`, the 2<sup>nd</sup> smallest negative integer is -1. 

For `[-2, -3]`, the 2<sup>nd</sup> smallest negative integer is -2. 

For `[-3, -4]`, the 2<sup>nd</sup> smallest negative integer is -3. 

For `[-4, -5]`, the 2<sup>nd</sup> smallest negative integer is -4.

**Example 3:**

**Input:** nums = [-3,1,2,-3,0,-3], k = 2, x = 1

**Output:** [-3,0,-3,-3,-3]

**Explanation:** There are 5 subarrays with size k = 2**.** 

For `[-3, 1]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[1, 2]`, there is no negative integer so the beauty is 0. 

For `[2, -3]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[-3, 0]`, the 1<sup>st</sup> smallest negative integer is -3. 

For `[0, -3]`, the 1<sup>st</sup> smallest negative integer is -3.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= k <= n`
*   `1 <= x <= k`
*   `-50 <= nums[i] <= 50`

## Solution

```kotlin
class Solution {
    fun getSubarrayBeauty(nums: IntArray, k: Int, x: Int): IntArray {
        val freqCounter = IntArray(50)
        var index = 0
        val results = IntArray(nums.size - k + 1)
        for (i in 0 until k) {
            if (nums[i] < 0) {
                freqCounter[nums[i] + 50]++
            }
        }
        results[index++] = getXthSmallest(freqCounter, x)
        while (index < results.size) {
            if (nums[index - 1] < 0) {
                freqCounter[nums[index - 1] + 50]--
            }
            if (nums[index + k - 1] < 0) {
                freqCounter[nums[index + k - 1] + 50]++
            }
            results[index++] = getXthSmallest(freqCounter, x)
        }
        return results
    }

    private fun getXthSmallest(freqCounter: IntArray, x: Int): Int {
        var count = 0
        for (i in 0..49) {
            count += freqCounter[i]
            if (count >= x) {
                return i - 50
            }
        }
        return 0
    }
}
```