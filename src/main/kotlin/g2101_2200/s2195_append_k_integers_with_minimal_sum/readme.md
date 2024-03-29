[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2195\. Append K Integers With Minimal Sum

Medium

You are given an integer array `nums` and an integer `k`. Append `k` **unique positive** integers that do **not** appear in `nums` to `nums` such that the resulting total sum is **minimum**.

Return _the sum of the_ `k` _integers appended to_ `nums`.

**Example 1:**

**Input:** nums = [1,4,25,10,25], k = 2

**Output:** 5

**Explanation:** The two unique positive integers that do not appear in nums which we append are 2 and 3.

The resulting sum of nums is 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70, which is the minimum.

The sum of the two integers appended is 2 + 3 = 5, so we return 5.

**Example 2:**

**Input:** nums = [5,6], k = 6

**Output:** 25

**Explanation:** The six unique positive integers that do not appear in nums which we append are 1, 2, 3, 4, 7, and 8.

The resulting sum of nums is 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36, which is the minimum.

The sum of the six integers appended is 1 + 2 + 3 + 4 + 7 + 8 = 25, so we return 25. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun minimalKSum(nums: IntArray, k: Int): Long {
        nums.sort()
        var sum: Long = 0
        var n = 0
        for (i in nums.indices) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                if (nums[i] - n > k) {
                    break
                }
                sum += nums[i].toLong()
                n++
            }
        }
        val t = n + k.toLong()
        return (1 + t) * t / 2 - sum
    }
}
```