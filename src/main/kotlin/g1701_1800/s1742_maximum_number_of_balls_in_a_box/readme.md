[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1742\. Maximum Number of Balls in a Box

Easy

You are working in a ball factory where you have `n` balls numbered from `lowLimit` up to `highLimit` **inclusive** (i.e., `n == highLimit - lowLimit + 1`), and an infinite number of boxes numbered from `1` to `infinity`.

Your job at this factory is to put each ball in the box with a number equal to the sum of digits of the ball's number. For example, the ball number `321` will be put in the box number `3 + 2 + 1 = 6` and the ball number `10` will be put in the box number `1 + 0 = 1`.

Given two integers `lowLimit` and `highLimit`, return _the number of balls in the box with the most balls._

**Example 1:**

**Input:** lowLimit = 1, highLimit = 10

**Output:** 2

**Explanation:** 

Box Number: 1 2 3 4 5 6 7 8 9 10 11 ...

Ball Count: 2 1 1 1 1 1 1 1 1 0 0 ... 

Box 1 has the most number of balls with 2 balls.

**Example 2:**

**Input:** lowLimit = 5, highLimit = 15

**Output:** 2

**Explanation:** 

Box Number: 1 2 3 4 5 6 7 8 9 10 11 ...

Ball Count: 1 1 1 1 2 2 1 1 1 0 0 ... 

Boxes 5 and 6 have the most number of balls with 2 balls in each.

**Example 3:**

**Input:** lowLimit = 19, highLimit = 28

**Output:** 2

**Explanation:**

Box Number: 1 2 3 4 5 6 7 8 9 10 11 12 ... 

Ball Count: 0 1 1 1 1 1 1 1 1 2 0 0 ... 

Box 10 has the most number of balls with 2 balls.

**Constraints:**

*   <code>1 <= lowLimit <= highLimit <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun countBalls(lowLimit: Int, highLimit: Int): Int {
        var maxValue: Int
        val countArray = IntArray(46)
        var currentSum: Int = getDigitSum(lowLimit)
        countArray[currentSum]++
        maxValue = 1
        for (i in lowLimit + 1..highLimit) {
            if (i % 10 == 0) {
                currentSum = getDigitSum(i)
            } else {
                currentSum++
            }
            countArray[currentSum]++
            if (countArray[currentSum] > maxValue) {
                maxValue = countArray[currentSum]
            }
        }
        return maxValue
    }

    private fun getDigitSum(num: Int): Int {
        var num: Int = num
        var currentSum = 0
        while (num > 0) {
            currentSum += num % 10
            num /= 10
        }
        return currentSum
    }
}
```