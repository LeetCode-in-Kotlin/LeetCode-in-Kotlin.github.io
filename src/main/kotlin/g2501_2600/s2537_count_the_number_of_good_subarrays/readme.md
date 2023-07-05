[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2537\. Count the Number of Good Subarrays

Medium

Given an integer array `nums` and an integer `k`, return _the number of **good** subarrays of_ `nums`.

A subarray `arr` is **good** if it there are **at least** `k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,1,1,1,1], k = 10

**Output:** 1

**Explanation:** The only good subarray is the array nums itself.

**Example 2:**

**Input:** nums = [3,1,4,3,2,2,4], k = 2

**Output:** 4

**Explanation:** There are 4 different good subarrays: 

- \[3,1,4,3,2,2] that has 2 pairs.

- \[3,1,4,3,2,2,4] that has 3 pairs.

- \[1,4,3,2,2,4] that has 2 pairs. 

- \[4,3,2,2,4] that has 2 pairs.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countGood(nums: IntArray, k: Int): Long {
        if (nums.size < 2) {
            return 0L
        }
        val countMap: MutableMap<Int, Int> = HashMap(nums.size, 0.99f)
        var goodSubArrays = 0L
        var current = 0L
        var left = 0
        var right = -1
        while (left < nums.size) {
            if (current < k) {
                if (++right == nums.size) {
                    break
                }
                val num = nums[right]
                var count = countMap[num]
                if (count == null) {
                    count = 1
                } else {
                    current += count.toLong()
                    if (current >= k) {
                        goodSubArrays += (nums.size - right).toLong()
                    }
                    count = count + 1
                }
                countMap[num] = count
            } else {
                val num = nums[left++]
                val count = countMap[num]!! - 1
                if (count > 0) {
                    countMap[num] = count
                    current -= count.toLong()
                } else {
                    countMap.remove(num)
                }
                if (current >= k) {
                    goodSubArrays += (nums.size - right).toLong()
                }
            }
        }
        return goodSubArrays
    }
}
```