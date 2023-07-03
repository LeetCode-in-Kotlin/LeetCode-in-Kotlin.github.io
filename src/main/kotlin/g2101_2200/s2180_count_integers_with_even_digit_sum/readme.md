[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2180\. Count Integers With Even Digit Sum

Easy

Given a positive integer `num`, return _the number of positive integers **less than or equal to**_ `num` _whose digit sums are **even**_.

The **digit sum** of a positive integer is the sum of all its digits.

**Example 1:**

**Input:** num = 4

**Output:** 2

**Explanation:** 

The only integers less than or equal to 4 whose digit sums are even are 2 and 4.

**Example 2:**

**Input:** num = 30

**Output:** 14

**Explanation:** 

The 14 integers less than or equal to 30 whose digit sums are even are 

2, 4, 6, 8, 11, 13, 15, 17, 19, 20, 22, 24, 26, and 28.

**Constraints:**

* `1 <= num <= 1000`

## Solution

```kotlin
class Solution {
    fun countEven(num: Int): Int {
        // Digit sum of the last number, we can get each digit this way sicne the range is [1, 1000]
        val sum = num % 10 + num / 10 % 10 + num / 100 % 10 + num / 1000 % 10

        // Check the parity of the digit sum of the last number
        return (num - (sum and 1)) / 2
    }
}
```