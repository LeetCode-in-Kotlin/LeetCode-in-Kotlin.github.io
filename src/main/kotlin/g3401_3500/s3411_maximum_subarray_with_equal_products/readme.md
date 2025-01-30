[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3411\. Maximum Subarray With Equal Products

Easy

You are given an array of **positive** integers `nums`.

An array `arr` is called **product equivalent** if `prod(arr) == lcm(arr) * gcd(arr)`, where:

*   `prod(arr)` is the product of all elements of `arr`.
*   `gcd(arr)` is the GCD of all elements of `arr`.
*   `lcm(arr)` is the LCM of all elements of `arr`.

Return the length of the **longest** **product equivalent** subarray of `nums`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

The term `lcm(a, b)` denotes the **least common multiple** of `a` and `b`.

**Example 1:**

**Input:** nums = [1,2,1,2,1,1,1]

**Output:** 5

**Explanation:**

The longest product equivalent subarray is `[1, 2, 1, 1, 1]`, where `prod([1, 2, 1, 1, 1]) = 2`, `gcd([1, 2, 1, 1, 1]) = 1`, and `lcm([1, 2, 1, 1, 1]) = 2`.

**Example 2:**

**Input:** nums = [2,3,4,5,6]

**Output:** 3

**Explanation:**

The longest product equivalent subarray is `[3, 4, 5].`

**Example 3:**

**Input:** nums = [1,2,3,1,4,5,1]

**Output:** 5

**Constraints:**

*   `2 <= nums.length <= 100`
*   `1 <= nums[i] <= 10`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) a else gcd(b, a % b)
    }

    private fun lcm(a: Int, b: Int): Int {
        return (a / gcd(a, b)) * b
    }

    fun maxLength(nums: IntArray): Int {
        val n = nums.size
        var maxL = 0
        for (i in 0..<n) {
            var currGCD = nums[i]
            var currLCM = nums[i]
            var currPro = nums[i]
            for (j in i + 1..<n) {
                currPro *= nums[j]
                currGCD = gcd(currGCD, nums[j])
                currLCM = lcm(currLCM, nums[j])
                if (currPro == currLCM * currGCD) {
                    maxL = max(maxL.toDouble(), (j - i + 1).toDouble()).toInt()
                }
            }
        }
        return maxL
    }
}
```