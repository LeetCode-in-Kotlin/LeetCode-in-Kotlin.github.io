[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2750\. Ways to Split Array Into Good Subarrays

Medium

You are given a binary array `nums`.

A subarray of an array is **good** if it contains **exactly** **one** element with the value `1`.

Return _an integer denoting the number of ways to split the array_ `nums` _into **good** subarrays_. As the number may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [0,1,0,0,1]

**Output:** 3

**Explanation:** There are 3 ways to split nums into good subarrays:
- \[0,1] [0,0,1] 
- \[0,1,0] [0,1]
- \[0,1,0,0] [1]

**Example 2:**

**Input:** nums = [0,1,0]

**Output:** 1

**Explanation:** There is 1 way to split nums into good subarrays: - [0,1,0]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `0 <= nums[i] <= 1`

## Solution

```kotlin
class Solution {
    fun numberOfGoodSubarraySplits(nums: IntArray): Int {
        val res: MutableList<Int> = ArrayList()
        val modulo = 1000000007L
        for (i in nums.indices) {
            if (nums[i] == 1) res.add(i)
        }
        var ans: Long = 0
        if (res.isNotEmpty()) ans = 1
        var kanishk = ans
        for (i in res.size - 2 downTo 0) {
            val leftInd = res[i]
            val rightInd = res[i + 1]
            val df = rightInd - leftInd
            val mul = df.toLong() % modulo * kanishk % modulo % modulo
            kanishk = mul
            ans = mul
        }
        return ans.toInt()
    }
}
```