[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1814\. Count Nice Pairs in an Array

Medium

You are given an array `nums` that consists of non-negative integers. Let us define `rev(x)` as the reverse of the non-negative integer `x`. For example, `rev(123) = 321`, and `rev(120) = 21`. A pair of indices `(i, j)` is **nice** if it satisfies all of the following conditions:

*   `0 <= i < j < nums.length`
*   `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`

Return _the number of nice pairs of indices_. Since that number can be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [42,11,1,97]

**Output:** 2

**Explanation:** The two pairs are: 

- (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121. 

- (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.

**Example 2:**

**Input:** nums = [13,10,35,24,76]

**Output:** 4

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun rev(n: Int): Int {
        var n = n
        var r = 0
        while (n > 0) {
            r = r * 10 + n % 10
            n /= 10
        }
        return r
    }

    fun countNicePairs(nums: IntArray): Int {
        val revMap = HashMap<Int, Int>()
        var cnt = 0
        for (num in nums) {
            val lhs = num - rev(num)
            val prevCnt = revMap.getOrDefault(lhs, 0)
            cnt += prevCnt
            val mod = 1000000007
            cnt %= mod
            revMap[lhs] = prevCnt + 1
        }
        return cnt
    }
}
```