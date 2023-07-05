[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2470\. Number of Subarrays With LCM Equal to K

Medium

Given an integer array `nums` and an integer `k`, return _the number of **subarrays** of_ `nums` _where the least common multiple of the subarray's elements is_ `k`.

A **subarray** is a contiguous non-empty sequence of elements within an array.

The **least common multiple of an array** is the smallest positive integer that is divisible by all the array elements.

**Example 1:**

**Input:** nums = [3,6,2,7,1], k = 6

**Output:** 4

**Explanation:** The subarrays of nums where 6 is the least common multiple of all the subarray's elements are:
- \[<ins>**3**</ins>,<ins>**6**</ins>,2,7,1] 
- \[<ins>**3**</ins>,<ins>**6**</ins>,<ins>**2**</ins>,7,1] 
- \[3,<ins>**6**</ins>,2,7,1] 
- \[3,<ins>**6**</ins>,<ins>**2**</ins>,7,1]

**Example 2:**

**Input:** nums = [3], k = 2

**Output:** 0

**Explanation:** There are no subarrays of nums where 2 is the least common multiple of all the subarray's elements.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i], k <= 1000`

## Solution

```kotlin
class Solution {
    fun subarrayLCM(nums: IntArray, k: Int): Int {
        var ans = 0
        for (i in nums.indices) {
            var lcm = nums[i]
            for (j in i until nums.size) {
                lcm = lcm * nums[j] / gcd(lcm, nums[j])
                if (lcm == k) {
                    ans++
                }
                if (k % lcm != 0) {
                    break
                }
            }
        }
        return ans
    }

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) a else gcd(b, a % b)
    }
}
```