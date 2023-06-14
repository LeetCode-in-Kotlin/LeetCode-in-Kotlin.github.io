[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1590\. Make Sum Divisible by P

Medium

Given an array of positive integers `nums`, remove the **smallest** subarray (possibly **empty**) such that the **sum** of the remaining elements is divisible by `p`. It is **not** allowed to remove the whole array.

Return _the length of the smallest subarray that you need to remove, or_ `-1` _if it's impossible_.

A **subarray** is defined as a contiguous block of elements in the array.

**Example 1:**

**Input:** nums = [3,1,4,2], p = 6

**Output:** 1

**Explanation:** The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.

**Example 2:**

**Input:** nums = [6,3,5,2], p = 9

**Output:** 2

**Explanation:** We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.

**Example 3:**

**Input:** nums = [1,2,3], p = 3

**Output:** 0

**Explanation:** Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= p <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minSubarray(nums: IntArray, p: Int): Int {
        val hmp = HashMap<Int, Int>()
        val n = nums.size
        var target = 0
        var sum = 0
        for (num in nums) {
            target = (num + target) % p
        }
        if (target == 0) {
            return 0
        }
        hmp[0] = -1
        var ans = n
        for (i in 0 until n) {
            sum = (sum + nums[i]) % p
            val key = (sum - target + p) % p
            if (hmp.containsKey(key)) {
                ans = Math.min(ans, i - hmp[key]!!)
            }
            hmp[sum % p] = i
        }
        return if (ans < n) {
            ans
        } else {
            -1
        }
    }
}
```