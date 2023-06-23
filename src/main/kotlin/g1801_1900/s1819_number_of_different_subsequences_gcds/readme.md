[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1819\. Number of Different Subsequences GCDs

Hard

You are given an array `nums` that consists of positive integers.

The **GCD** of a sequence of numbers is defined as the greatest integer that divides **all** the numbers in the sequence evenly.

*   For example, the GCD of the sequence `[4,6,16]` is `2`.

A **subsequence** of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

*   For example, `[2,5,10]` is a subsequence of `[1,2,1,**2**,4,1,**5**,**10**]`.

Return _the **number** of **different** GCDs among all **non-empty** subsequences of_ `nums`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/17/image-1.png)

**Input:** nums = [6,10,3]

**Output:** 5

**Explanation:** The figure shows all the non-empty subsequences and their GCDs. The different GCDs are 6, 10, 3, 2, and 1.

**Example 2:**

**Input:** nums = [5,15,40,5,6]

**Output:** 7

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 2 * 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun countDifferentSubsequenceGCDs(nums: IntArray): Int {
        var max = 0
        for (num in nums) {
            max = max.coerceAtLeast(num)
        }
        val present = BooleanArray(200001)
        for (num in nums) {
            max = max.coerceAtLeast(num)
            present[num] = true
        }
        var count = 0
        for (i in 1..max) {
            if (present[i]) {
                count++
                continue
            }
            var tempGcd = 0
            var j = i
            while (j <= max) {
                if (present[j]) {
                    tempGcd = gcd(tempGcd, j)
                }
                if (tempGcd == i) {
                    count++
                    break
                }
                j += i
            }
        }
        return count
    }

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) {
            a
        } else gcd(b, a % b)
    }
}
```