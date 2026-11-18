[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4010\. Maximize Pair Strength Using GCD

Easy

You are given an integer array `nums`.

Choose exactly one pair of distinct indices `i` and `j`. The strength of the pair is defined as <code>(nums[i] * nums[j]) / gcd(nums[i], nums[j])<sup>2</sup></code>.

Return the maximum strength over all possible pairs.

**Example 1:**

**Input:** nums = [2,3,5]

**Output:** 15

**Explanation:**

Choosing `i = 1` and `j = 2` gives strength <code>(3 * 5) / gcd(3, 5)<sup>2</sup> = 15 / 1 = 15</code>, which is the maximum over all pairs.

**Example 2:**

**Input:** nums = [4,6,8]

**Output:** 12

**Explanation:**

Choosing `i = 1` and `j = 2` gives strength <code>(6 * 8) / gcd(6, 8)<sup>2</sup> = 48 / 4 = 12</code>, which is the maximum over all pairs.

**Example 3:**

**Input:** nums = [3,3]

**Output:** 1

**Explanation:**

Choosing `i = 0` and `j = 1` gives strength <code>(3 * 3) / gcd(3, 3)<sup>2</sup> = 9 / 9 = 1</code>, the maximum over all pairs.

**Constraints:**

*   `2 <= nums.length <= 2000`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    private fun gcd(n: Int, m: Int): Int {
        var n = n
        var m = m
        while (m != 0) {
            val remainder = n % m
            n = m
            m = remainder
        }
        return n
    }

    fun maxPairStrength(nums: IntArray): Long {
        var maxStrength: Long = 0
        val end = nums.size - 1
        nums.sort()
        for (i in end downTo 1) {
            for (j in i - 1 downTo 0) {
                val product = nums[i].toLong() * nums[j]
                if (product <= maxStrength) {
                    break
                }
                val hcf = gcd(nums[i], nums[j])
                if (hcf == 0) {
                    continue
                }
                val currentStrength = (nums[i] / hcf).toLong() * (nums[j] / hcf)
                if (currentStrength > maxStrength) {
                    maxStrength = currentStrength
                }
            }
        }
        return maxStrength
    }
}
```