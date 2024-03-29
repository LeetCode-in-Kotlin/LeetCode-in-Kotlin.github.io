[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1295\. Find Numbers with Even Number of Digits

Easy

Given an array `nums` of integers, return how many of them contain an **even number** of digits.

**Example 1:**

**Input:** nums = [12,345,2,6,7896]

**Output:** 2

**Explanation:**

12 contains 2 digits (even number of digits).

345 contains 3 digits (odd number of digits).

2 contains 1 digit (odd number of digits).

6 contains 1 digit (odd number of digits).

7896 contains 4 digits (even number of digits).

Therefore only 12 and 7896 contain an even number of digits.

**Example 2:**

**Input:** nums = [555,901,482,1771]

**Output:** 1

**Explanation:**  Only 1771 contains an even number of digits.

**Constraints:**

*   `1 <= nums.length <= 500`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun findNumbers(nums: IntArray): Int {
        // initialising variable to hold number of digits and numbers having even number of digits
        var digitCount = 0
        var evendigitCount = 0
        // traversing through the array
        for (num in nums) {
            var number = num
            while (number != 0) {
                // counting digits for each number
                digitCount++
                number /= 10
            }
            // incrementing variable for numbers having even number of digits
            if (digitCount % 2 == 0) {
                evendigitCount++
            }
            // reassigning the value to reset digits for next number in iteration
            digitCount = 0
        }
        return evendigitCount
    }
}
```