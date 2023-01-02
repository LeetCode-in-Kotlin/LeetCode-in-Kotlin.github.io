[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 477\. Total Hamming Distance

Medium

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.

Given an integer array `nums`, return _the sum of **Hamming distances** between all the pairs of the integers in_ `nums`.

**Example 1:**

**Input:** nums = [4,14,2]

**Output:** 6

**Explanation:** In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just showing the four bits relevant in this case). 

The answer will be: 

HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.

**Example 2:**

**Input:** nums = [4,14,4]

**Output:** 4

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   The answer for the given input will fit in a **32-bit** integer.

## Solution

```kotlin
class Solution {
    fun totalHammingDistance(nums: IntArray): Int {
        var ans = 0
        val n = nums.size
        for (i in 0..31) {
            var ones = 0
            for (k in nums) {
                ones += k shr i and 1
            }
            ans = ans + ones * (n - ones)
        }
        return ans
    }
}
```