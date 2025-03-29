[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2815\. Max Pair Sum in an Array

Easy

You are given a **0-indexed** integer array `nums`. You have to find the **maximum** sum of a pair of numbers from `nums` such that the maximum **digit** in both numbers are equal.

Return _the maximum sum or_ `-1` _if no such pair exists_.

**Example 1:**

**Input:** nums = [51,71,17,24,42]

**Output:** 88

**Explanation:**

For i = 1 and j = 2, nums[i] and nums[j] have equal maximum digits with a pair sum of 71 + 17 = 88.

For i = 3 and j = 4, nums[i] and nums[j] have equal maximum digits with a pair sum of 24 + 42 = 66.

It can be shown that there are no other pairs with equal maximum digits, so the answer is 88.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** -1

**Explanation:** No pair exists in nums with equal maximum digits. 

**Constraints:**

*   `2 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue
import kotlin.math.max

@Suppress("NAME_SHADOWING")
class Solution {
    fun maxSum(nums: IntArray): Int {
        // what we'll return
        var maxSum = -1
        val maximumDigitToNumber: MutableMap<Int, PriorityQueue<Int>> = HashMap()
        for (i in 1..9) {
            maximumDigitToNumber[i] = PriorityQueue(Comparator.reverseOrder())
        }
        for (n in nums) {
            maximumDigitToNumber[getMaximumDigit(n)]!!.add(n)
        }
        for ((_, value) in maximumDigitToNumber) {
            if (value.size <= 1) {
                continue
            }
            val sum = value.poll() + value.poll()
            maxSum = max(maxSum, sum)
        }
        return maxSum
    }

    private fun getMaximumDigit(n: Int): Int {
        var n = n
        var maxDigit = 1
        var nMod10 = n % 10
        while (n > 0) {
            maxDigit = max(maxDigit.toDouble(), nMod10.toDouble()).toInt()
            n /= 10
            nMod10 = n % 10
        }
        return maxDigit
    }
}
```