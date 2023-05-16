[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 995\. Minimum Number of K Consecutive Bit Flips

Hard

You are given a binary array `nums` and an integer `k`.

A **k-bit flip** is choosing a **subarray** of length `k` from `nums` and simultaneously changing every `0` in the subarray to `1`, and every `1` in the subarray to `0`.

Return _the minimum number of **k-bit flips** required so that there is no_ `0` _in the array_. If it is not possible, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [0,1,0], k = 1

**Output:** 2

**Explanation:** Flip nums[0], then flip nums[2].

**Example 2:**

**Input:** nums = [1,1,0], k = 2

**Output:** -1

**Explanation:** No matter how we flip subarrays of size 2, we cannot make the array become [1,1,1].

**Example 3:**

**Input:** nums = [0,0,0,1,0,1,1,0], k = 3

**Output:** 3

**Explanation:** 

Flip nums[0],nums[1],nums[2]: nums becomes [1,1,1,1,0,1,1,0] 

Flip nums[4],nums[5],nums[6]: nums becomes [1,1,1,1,1,0,0,0] 

Flip nums[5],nums[6],nums[7]: nums becomes [1,1,1,1,1,1,1,1]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `1 <= k <= nums.length`

## Solution

```kotlin
class Solution {
    fun minKBitFlips(nums: IntArray, k: Int): Int {
        val n = nums.size
        val pref = IntArray(n)
        for (i in 0 until n) {
            if (i == 0) {
                if (nums[i] == 0) {
                    pref[i]++
                }
            } else {
                pref[i] = pref[i - 1]
                val flips = pref[i] - if (i - k >= 0) pref[i - k] else 0
                if (flips % 2 == nums[i]) {
                    if (i + k > n) {
                        return -1
                    }
                    pref[i]++
                }
            }
        }
        return pref[n - 1]
    }
}
```