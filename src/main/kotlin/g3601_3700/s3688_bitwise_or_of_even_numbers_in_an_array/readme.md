[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3688\. Bitwise OR of Even Numbers in an Array

Easy

You are given an integer array `nums`.

Return the bitwise **OR** of all **even** numbers in the array.

If there are no even numbers in `nums`, return 0.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6]

**Output:** 6

**Explanation:**

The even numbers are 2, 4, and 6. Their bitwise OR equals 6.

**Example 2:**

**Input:** nums = [7,9,11]

**Output:** 0

**Explanation:**

There are no even numbers, so the result is 0.

**Example 3:**

**Input:** nums = [1,8,16]

**Output:** 24

**Explanation:**

The even numbers are 8 and 16. Their bitwise OR equals 24.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun evenNumberBitwiseORs(nums: IntArray): Int {
        var count = 0
        for (num in nums) {
            if (num % 2 == 0) {
                count = count or num
            }
        }
        return count
    }
}
```