[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2455\. Average Value of Even Numbers That Are Divisible by Three

Easy

Given an integer array `nums` of **positive** integers, return _the average value of all even integers that are divisible by_ `3`_._

Note that the **average** of `n` elements is the **sum** of the `n` elements divided by `n` and **rounded down** to the nearest integer.

**Example 1:**

**Input:** nums = [1,3,6,10,12,15]

**Output:** 9

**Explanation:** 6 and 12 are even numbers that are divisible by 3. (6 + 12) / 2 = 9.

**Example 2:**

**Input:** nums = [1,2,4,7,10]

**Output:** 0

**Explanation:** There is no single number that satisfies the requirement, so return 0.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun averageValue(nums: IntArray): Int {
        var count = 0
        var sum = 0
        for (num in nums) {
            if (num % 2 == 0 && num % 3 == 0) {
                count++
                sum += num
            }
        }
        return if (count == 0) {
            0
        } else sum / count
    }
}
```