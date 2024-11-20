[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2447\. Number of Subarrays With GCD Equal to K

Medium

Given an integer array `nums` and an integer `k`, return _the number of **subarrays** of_ `nums` _where the greatest common divisor of the subarray's elements is_ `k`.

A **subarray** is a contiguous non-empty sequence of elements within an array.

The **greatest common divisor of an array** is the largest integer that evenly divides all the array elements.

**Example 1:**

**Input:** nums = [9,3,1,2,6,3], k = 3

**Output:** 4

**Explanation:** The subarrays of nums where 3 is the greatest common divisor of all the subarray's elements are:

- \[9,<ins>**3**</ins>,1,2,6,3] 
 
- \[9,3,1,2,6,<ins>**3**</ins>] 
 
- \[<ins>**9,3**</ins>,1,2,6,3] 
 
- \[9,3,1,2,<ins>**6,3**</ins>]

**Example 2:**

**Input:** nums = [4], k = 7

**Output:** 0

**Explanation:** There are no subarrays of nums where 7 is the greatest common divisor of all the subarray's elements.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i], k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private fun sol(a: Int, b: Int): Int {
        return if (b == 0) {
            a
        } else {
            sol(b, a % b)
        }
    }

    fun subarrayGCD(nums: IntArray, k: Int): Int {
        val n = nums.size
        var cnt = 0
        for (i in 0 until n) {
            var gcd = 0
            for (j in i until n) {
                gcd = sol(gcd, nums[j])
                if (gcd == k) {
                    cnt++
                }
                if (gcd < k) {
                    break
                }
            }
        }
        return cnt
    }
}
```