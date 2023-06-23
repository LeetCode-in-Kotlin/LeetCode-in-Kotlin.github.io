[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1862\. Sum of Floored Pairs

Hard

Given an integer array `nums`, return the sum of `floor(nums[i] / nums[j])` for all pairs of indices `0 <= i, j < nums.length` in the array. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

The `floor()` function returns the integer part of the division.

**Example 1:**

**Input:** nums = [2,5,9]

**Output:** 10

**Explanation:** 

floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0 

floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1 

floor(5 / 2) = 2 

floor(9 / 2) = 4 

floor(9 / 5) = 1 

We calculate the floor of the division for every pair of indices in the array then sum them up.

**Example 2:**

**Input:** nums = [7,7,7,7,7,7,7]

**Output:** 49

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun sumOfFlooredPairs(nums: IntArray): Int {
        val mod: Long = 1000000007
        nums.sort()
        val max = nums[nums.size - 1]
        val counts = IntArray(max + 1)
        val qnts = LongArray(max + 1)
        for (k in nums) {
            counts[k]++
        }
        for (i in 1 until max + 1) {
            if (counts[i] == 0) {
                continue
            }
            var j = i
            while (j <= max) {
                qnts[j] += counts[i].toLong()
                j = j + i
            }
        }
        for (i in 1 until max + 1) {
            qnts[i] = (qnts[i] + qnts[i - 1]) % mod
        }
        var sum: Long = 0
        for (k in nums) {
            sum = (sum + qnts[k]) % mod
        }
        return sum.toInt()
    }
}
```