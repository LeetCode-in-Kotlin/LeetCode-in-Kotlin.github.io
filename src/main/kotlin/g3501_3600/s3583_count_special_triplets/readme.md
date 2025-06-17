[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3583\. Count Special Triplets

Medium

You are given an integer array `nums`.

A **special triplet** is defined as a triplet of indices `(i, j, k)` such that:

*   `0 <= i < j < k < n`, where `n = nums.length`
*   `nums[i] == nums[j] * 2`
*   `nums[k] == nums[j] * 2`

Return the total number of **special triplets** in the array.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [6,3,6]

**Output:** 1

**Explanation:**

The only special triplet is `(i, j, k) = (0, 1, 2)`, where:

*   `nums[0] = 6`, `nums[1] = 3`, `nums[2] = 6`
*   `nums[0] = nums[1] * 2 = 3 * 2 = 6`
*   `nums[2] = nums[1] * 2 = 3 * 2 = 6`

**Example 2:**

**Input:** nums = [0,1,0,0]

**Output:** 1

**Explanation:**

The only special triplet is `(i, j, k) = (0, 2, 3)`, where:

*   `nums[0] = 0`, `nums[2] = 0`, `nums[3] = 0`
*   `nums[0] = nums[2] * 2 = 0 * 2 = 0`
*   `nums[3] = nums[2] * 2 = 0 * 2 = 0`

**Example 3:**

**Input:** nums = [8,4,2,8,4]

**Output:** 2

**Explanation:**

There are exactly two special triplets:

*   `(i, j, k) = (0, 1, 3)`
    *   `nums[0] = 8`, `nums[1] = 4`, `nums[3] = 8`
    *   `nums[0] = nums[1] * 2 = 4 * 2 = 8`
    *   `nums[3] = nums[1] * 2 = 4 * 2 = 8`
*   `(i, j, k) = (1, 2, 4)`
    *   `nums[1] = 4`, `nums[2] = 2`, `nums[4] = 4`
    *   `nums[1] = nums[2] * 2 = 2 * 2 = 4`
    *   `nums[4] = nums[2] * 2 = 2 * 2 = 4`

**Constraints:**

*   <code>3 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun specialTriplets(nums: IntArray): Int {
        val mod = 1_000_000_007
        var res = 0
        val left = mutableMapOf<Int, Int>()
        val right = mutableMapOf<Int, Int>()
        for (num in nums) {
            right[num] = right.getOrDefault(num, 0) + 1
        }
        for (num in nums) {
            right[num] = right[num]!! - 1
            val ci = left.getOrDefault(num * 2, 0)
            val ck = right.getOrDefault(num * 2, 0)
            res = ((res + 1L * ci * ck) % mod).toInt()
            left[num] = left.getOrDefault(num, 0) + 1
        }
        return res
    }
}
```