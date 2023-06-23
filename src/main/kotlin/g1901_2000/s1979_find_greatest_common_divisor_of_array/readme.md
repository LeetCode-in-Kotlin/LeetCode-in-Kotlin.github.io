[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1979\. Find Greatest Common Divisor of Array

Easy

Given an integer array `nums`, return _the **greatest common divisor** of the smallest number and largest number in_ `nums`.

The **greatest common divisor** of two numbers is the largest positive integer that evenly divides both numbers.

**Example 1:**

**Input:** nums = [2,5,6,9,10]

**Output:** 2

**Explanation:** 

The smallest number in nums is 2. 

The largest number in nums is 10. 

The greatest common divisor of 2 and 10 is 2.

**Example 2:**

**Input:** nums = [7,5,6,8,3]

**Output:** 1

**Explanation:** 

The smallest number in nums is 3. 

The largest number in nums is 8. 

The greatest common divisor of 3 and 8 is 1.

**Example 3:**

**Input:** nums = [3,3]

**Output:** 3

**Explanation:** 

The smallest number in nums is 3. 

The largest number in nums is 3. 

The greatest common divisor of 3 and 3 is 3.

**Constraints:**

*   `2 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun findGCD(nums: IntArray): Int {
        var max = Int.MIN_VALUE
        var min = Int.MAX_VALUE
        for (num in nums) {
            if (max < num) {
                max = num
            }
            if (min > num) {
                min = num
            }
        }
        return findGCD(max, min)
    }

    private fun findGCD(x: Int, y: Int): Int {
        var r: Int
        var a: Int
        var b: Int
        a = if (x > y) x else y
        b = if (x < y) x else y
        r = b
        while (a % b != 0) {
            r = a % b
            a = b
            b = r
        }
        return r
    }
}
```