[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2183\. Count Array Pairs Divisible by K

Hard

Given a **0-indexed** integer array `nums` of length `n` and an integer `k`, return _the **number of pairs**_ `(i, j)` _such that:_

*   `0 <= i < j <= n - 1` _and_
*   `nums[i] * nums[j]` _is divisible by_ `k`.

**Example 1:**

**Input:** nums = [1,2,3,4,5], k = 2

**Output:** 7

**Explanation:**

The 7 pairs of indices whose corresponding products are divisible by 2 are

(0, 1), (0, 3), (1, 2), (1, 3), (1, 4), (2, 3), and (3, 4).

Their products are 2, 4, 6, 8, 10, 12, and 20 respectively.

Other pairs such as (0, 2) and (2, 4) have products 3 and 15 respectively, which are not divisible by 2. 

**Example 2:**

**Input:** nums = [1,2,3,4], k = 5

**Output:** 0

**Explanation:** There does not exist any pair of indices whose corresponding product is divisible by 5. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], k <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun countPairs(nums: IntArray, k: Int): Long {
        var count = 0L
        val map: MutableMap<Int, Long> = HashMap()
        for (num in nums) {
            val gd = gcd(num, k)
            val want = k / gd
            for ((key, value) in map) {
                if (key % want == 0) {
                    count += value
                }
            }
            map[gd] = map.getOrDefault(gd, 0L) + 1L
        }
        return count
    }

    private fun gcd(a: Int, b: Int): Int {
        if (a > b) {
            return gcd(b, a)
        }
        return if (a == 0) {
            b
        } else {
            gcd(a, b % a)
        }
    }
}
```