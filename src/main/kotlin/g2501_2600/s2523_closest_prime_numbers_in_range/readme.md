[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2523\. Closest Prime Numbers in Range

Medium

Given two positive integers `left` and `right`, find the two integers `num1` and `num2` such that:

*   `left <= nums1 < nums2 <= right` .
*   `nums1` and `nums2` are both **prime** numbers.
*   `nums2 - nums1` is the **minimum** amongst all other pairs satisfying the above conditions.

Return _the positive integer array_ `ans = [nums1, nums2]`. _If there are multiple pairs satisfying these conditions, return the one with the minimum_ `nums1` _value or_ `[-1, -1]` _if such numbers do not exist._

A number greater than `1` is called **prime** if it is only divisible by `1` and itself.

**Example 1:**

**Input:** left = 10, right = 19

**Output:** [11,13]

**Explanation:** The prime numbers between 10 and 19 are 11, 13, 17, and 19. 

The closest gap between any pair is 2, which can be achieved by [11,13] or [17,19]. 

Since 11 is smaller than 17, we return the first pair.

**Example 2:**

**Input:** left = 4, right = 6

**Output:** [-1,-1]

**Explanation:** There exists only one prime number in the given range, so the conditions cannot be satisfied.

**Constraints:**

*   <code>1 <= left <= right <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun closestPrimes(left: Int, right: Int): IntArray {
        var diff = -1
        var x = -1
        var y = -1
        var prev = -1
        for (i in left..right) {
            val isPrime = isPrime(i)
            if (isPrime) {
                if (prev != -1) {
                    val d = i - prev
                    if (diff == -1 || d < diff) {
                        diff = d
                        x = prev
                        y = i
                        if (diff <= 2) {
                            return intArrayOf(x, y)
                        }
                    }
                }
                prev = i
            }
        }
        return intArrayOf(x, y)
    }

    private fun isPrime(x: Int): Boolean {
        if (x == 1) {
            return false
        }
        val sqrt = Math.sqrt(x.toDouble())
        var i = 2
        while (i <= sqrt) {
            if (x % i == 0) {
                return false
            }
            i++
        }
        return true
    }
}
```