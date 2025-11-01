[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3729\. Count Distinct Subarrays Divisible by K in Sorted Array

Hard

You are given an integer array `nums` **sorted** in **non-descending** order and a positive integer `k`.

A ****non-empty subarrays**** of `nums` is **good** if the sum of its elements is **divisible** by `k`.

Return an integer denoting the number of **distinct** **good** subarrays of `nums`.

Subarrays are **distinct** if their sequences of values are. For example, there are 3 **distinct** subarrays in `[1, 1, 1]`, namely `[1]`, `[1, 1]`, and `[1, 1, 1]`.

**Example 1:**

**Input:** nums = [1,2,3], k = 3

**Output:** 3

**Explanation:**

The good subarrays are `[1, 2]`, `[3]`, and `[1, 2, 3]`. For example, `[1, 2, 3]` is good because the sum of its elements is `1 + 2 + 3 = 6`, and `6 % k = 6 % 3 = 0`.

**Example 2:**

**Input:** nums = [2,2,2,2,2,2], k = 6

**Output:** 2

**Explanation:**

The good subarrays are `[2, 2, 2]` and `[2, 2, 2, 2, 2, 2]`. For example, `[2, 2, 2]` is good because the sum of its elements is `2 + 2 + 2 = 6`, and `6 % k = 6 % 6 = 0`.

Note that `[2, 2, 2]` is counted only once.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `nums` is sorted in non-descending order.
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun numGoodSubarrays(nums: IntArray, k: Int): Long {
        val cnt: MutableMap<Int, Int> = HashMap(nums.size, 1f)
        cnt[0] = 1
        var sum: Long = 0
        var lastStart = 0
        var ans: Long = 0
        for (i in nums.indices) {
            val x = nums[i]
            if (i > 0 && x != nums[i - 1]) {
                var s = sum
                for (t in i - lastStart downTo 1) {
                    cnt.merge((s % k).toInt(), 1) { a: Int, b: Int -> Integer.sum(a, b) }
                    s -= nums[i - 1].toLong()
                }
                lastStart = i
            }
            sum += x.toLong()
            ans += cnt.getOrDefault((sum % k).toInt(), 0).toLong()
        }
        return ans
    }
}
```