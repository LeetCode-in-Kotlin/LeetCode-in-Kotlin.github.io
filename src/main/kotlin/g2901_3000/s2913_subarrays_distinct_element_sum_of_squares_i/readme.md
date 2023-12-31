[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2913\. Subarrays Distinct Element Sum of Squares I

Easy

You are given a **0-indexed** integer array `nums`.

The **distinct count** of a subarray of `nums` is defined as:

*   Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the **squares** of **distinct counts** of all subarrays of_ `nums`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** 15

**Explanation:** Six possible subarrays are: 

[1]: 1 distinct value 

[2]: 1 distinct value 

[1]: 1 distinct value 

[1,2]: 2 distinct values 

[2,1]: 2 distinct values 

[1,2,1]: 2 distinct values 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.

**Example 2:**

**Input:** nums = [1,1]

**Output:** 3

**Explanation:** Three possible subarrays are: 

[1]: 1 distinct value 

[1]: 1 distinct value 

[1,1]: 1 distinct value 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun sumCounts(nums: List<Int>): Int {
        val n = nums.size
        if (n == 1) {
            return 1
        }
        val numsArr = IntArray(n)
        for (i in 0 until n) {
            numsArr[i] = nums[i]
        }
        val prev = IntArray(n)
        val foundAt = IntArray(101)
        var dupFound = false
        var j = 0
        while (j < n) {
            if (((foundAt[numsArr[j]] - 1).also { prev[j] = it }) >= 0) {
                dupFound = true
            }
            foundAt[numsArr[j]] = ++j
        }
        if (!dupFound) {
            return (((((n + 4) * n + 5) * n) + 2) * n) / 12
        }
        var result = 0
        for (start in n - 1 downTo 0) {
            var distinctCount = 0
            for (i in start until n) {
                if (prev[i] < start) {
                    distinctCount++
                }
                result += distinctCount * distinctCount
            }
        }
        return result
    }
}
```