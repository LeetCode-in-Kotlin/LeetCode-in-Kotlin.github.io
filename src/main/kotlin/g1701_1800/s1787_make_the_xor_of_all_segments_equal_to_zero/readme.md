[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1787\. Make the XOR of All Segments Equal to Zero

Hard

You are given an array `nums` and an integer `k`. The XOR of a segment `[left, right]` where `left <= right` is the `XOR` of all the elements with indices between `left` and `right`, inclusive: `nums[left] XOR nums[left+1] XOR ... XOR nums[right]`.

Return _the minimum number of elements to change in the array_ such that the `XOR` of all segments of size `k` is equal to zero.

**Example 1:**

**Input:** nums = [1,2,0,3,0], k = 1

**Output:** 3

**Explanation:** Modify the array from [**1**,**2**,0,**3**,0] to from [**0**,**0**,0,**0**,0]. 

**Example 2:**

**Input:** nums = [3,4,5,2,1,7,3,4,7], k = 3

**Output:** 3

**Explanation:** Modify the array from [3,4,**5**,**2**,**1**,7,3,4,7] to [3,4,**7**,**3**,**4**,7,3,4,7]. 

**Example 3:**

**Input:** nums = [1,2,4,1,2,5,1,2,6], k = 3

**Output:** 3

**Explanation:** Modify the array from [1,2,**4,**1,2,**5**,1,2,**6**] to [1,2,**3**,1,2,**3**,1,2,**3**].

**Constraints:**

*   `1 <= k <= nums.length <= 2000`
*   <code>0 <= nums[i] < 2<sup>10</sup></code>

## Solution

```kotlin
class Solution {
    fun minChanges(nums: IntArray, k: Int): Int {
        val n = nums.size
        val fre = Array(k) { IntArray(1024) }
        for (i in 0 until n) {
            fre[i % k][nums[i]]++
        }
        var dp = IntArray(1024)
        dp.fill(-n)
        dp[0] = 0
        var max = 0
        for (i in 0 until k) {
            val dp2 = IntArray(1024)
            dp2.fill(max)
            var max2 = 0
            for (xor in 0..1023) {
                var al = i
                while (al < n) {
                    dp2[xor] = Math.max(dp2[xor], dp[xor xor nums[al]] + fre[i][nums[al]])
                    al += k
                }
                max2 = Math.max(max2, dp2[xor])
            }
            max = max2
            dp = dp2
        }
        return n - dp[0]
    }
}
```