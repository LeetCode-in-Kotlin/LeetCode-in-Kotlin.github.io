[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1390\. Four Divisors

Medium

Given an integer array `nums`, return _the sum of divisors of the integers in that array that have exactly four divisors_. If there is no such integer in the array, return `0`.

**Example 1:**

**Input:** nums = [21,4,7]

**Output:** 32

**Explanation:**

21 has 4 divisors: 1, 3, 7, 21

4 has 3 divisors: 1, 2, 4

7 has 2 divisors: 1, 7

The answer is the sum of divisors of 21 only.

**Example 2:**

**Input:** nums = [21,21]

**Output:** 64

**Example 3:**

**Input:** nums = [1,2,3,4,5]

**Output:** 0

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun sumFourDivisors(nums: IntArray): Int {
        var sum = 0
        for (num in nums) {
            val sqrt = Math.sqrt(num.toDouble()).toInt()
            if (sqrt * sqrt == num) {
                continue
            }
            var tmpSum = num + 1
            var count = 0
            for (i in 2..sqrt) {
                if (num % i == 0) {
                    count++
                    tmpSum += i + num / i
                }
                if (count > 1) {
                    break
                }
            }
            if (count == 1) {
                sum += tmpSum
            }
        }
        return sum
    }
}
```