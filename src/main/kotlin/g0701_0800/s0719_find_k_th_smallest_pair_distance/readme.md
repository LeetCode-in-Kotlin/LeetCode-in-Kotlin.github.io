[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 719\. Find K-th Smallest Pair Distance

Hard

The **distance of a pair** of integers `a` and `b` is defined as the absolute difference between `a` and `b`.

Given an integer array `nums` and an integer `k`, return _the_ <code>k<sup>th</sup></code> _smallest **distance among all the pairs**_ `nums[i]` _and_ `nums[j]` _where_ `0 <= i < j < nums.length`.

**Example 1:**

**Input:** nums = [1,3,1], k = 1

**Output:** 0

**Explanation:** Here are all the pairs: 

(1,3) -> 2 

(1,1) -> 0 

(3,1) -> 2 

Then the 1<sup>st</sup> smallest distance pair is (1,1), and its distance is 0.

**Example 2:**

**Input:** nums = [1,1,1], k = 2

**Output:** 0

**Example 3:**

**Input:** nums = [1,6,1], k = 3

**Output:** 5

**Constraints:**

*   `n == nums.length`
*   <code>2 <= n <= 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 10<sup>6</sup></code>
*   `1 <= k <= n * (n - 1) / 2`

## Solution

```kotlin
class Solution {
    fun smallestDistancePair(nums: IntArray, k: Int): Int {
        nums.sort()
        val length = nums.size
        val maxDiff = nums[length - 1] - nums[0]
        var start = 0
        var end = maxDiff
        while (start < end) {
            val mid = start + (end - start) / 2
            if (isPair(nums, mid, k)) {
                end = mid
            } else {
                start = mid + 1
            }
        }
        return start
    }

    private fun isPair(nums: IntArray, mid: Int, k: Int): Boolean {
        var count = 0
        var i = 0
        for (j in 1 until nums.size) {
            while (nums[j] - nums[i] > mid) {
                i++
            }
            count += j - i
        }
        return count >= k
    }
}
```