[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2652\. Sum Multiples

Easy

Given a positive integer `n`, find the sum of all integers in the range `[1, n]` **inclusive** that are divisible by `3`, `5`, or `7`.

Return _an integer denoting the sum of all numbers in the given range satisfying the constraint._

**Example 1:**

**Input:** n = 7

**Output:** 21

**Explanation:** Numbers in the range `[1, 7]` that are divisible by `3`, `5,` or `7` are `3, 5, 6, 7`. The sum of these numbers is `21`.

**Example 2:**

**Input:** n = 10

**Output:** 40

**Explanation:** Numbers in the range `[1, 10] that are` divisible by `3`, `5,` or `7` are `3, 5, 6, 7, 9, 10`. The sum of these numbers is 40.

**Example 3:**

**Input:** n = 9

**Output:** 30

**Explanation:** Numbers in the range `[1, 9]` that are divisible by `3`, `5`, or `7` are `3, 5, 6, 7, 9`. The sum of these numbers is `30`.

**Constraints:**

*   <code>1 <= n <= 10<sup>3</sup></code>

## Solution

```kotlin
class Solution {
    fun sumOfMultiples(n: Int): Int {
        fun sumOfDivisible(value: Int): Int {
            val high = n / value * value
            val count = (high + value - value) / value
            return (value + high) * count / 2
        }
        return sumOfDivisible(3) + sumOfDivisible(5) + sumOfDivisible(7) -
            (sumOfDivisible(15) + sumOfDivisible(35) + sumOfDivisible(21)) +
            sumOfDivisible(105)
    }
}
```