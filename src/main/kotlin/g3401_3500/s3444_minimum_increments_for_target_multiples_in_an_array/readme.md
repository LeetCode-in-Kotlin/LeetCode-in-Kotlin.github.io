[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3444\. Minimum Increments for Target Multiples in an Array

Hard

You are given two arrays, `nums` and `target`.

In a single operation, you may increment any element of `nums` by 1.

Return **the minimum number** of operations required so that each element in `target` has **at least** one multiple in `nums`.

**Example 1:**

**Input:** nums = [1,2,3], target = [4]

**Output:** 1

**Explanation:**

The minimum number of operations required to satisfy the condition is 1.

*   Increment 3 to 4 with just one operation, making 4 a multiple of itself.

**Example 2:**

**Input:** nums = [8,4], target = [10,5]

**Output:** 2

**Explanation:**

The minimum number of operations required to satisfy the condition is 2.

*   Increment 8 to 10 with 2 operations, making 10 a multiple of both 5 and 10.

**Example 3:**

**Input:** nums = [7,9,10], target = [7]

**Output:** 0

**Explanation:**

Target 7 already has a multiple in nums, so no additional operations are needed.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   `1 <= target.length <= 4`
*   `target.length <= nums.length`
*   <code>1 <= nums[i], target[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumIncrements(nums: IntArray, target: IntArray): Int {
        val m = target.size
        val fullMask = (1 shl m) - 1
        val lcmArr = LongArray(1 shl m)
        for (mask in 1..<(1 shl m)) {
            var l: Long = 1
            for (j in 0..<m) {
                if ((mask and (1 shl j)) != 0) {
                    l = lcm(l, target[j].toLong())
                }
            }
            lcmArr[mask] = l
        }
        val inf = Long.Companion.MAX_VALUE / 2
        var dp = LongArray(1 shl m)
        for (i in 0..<(1 shl m)) {
            dp[i] = inf
        }
        dp[0] = 0
        for (x in nums) {
            val newdp = dp.clone()
            for (mask in 1..<(1 shl m)) {
                val l = lcmArr[mask]
                val r = x % l
                val cost = (if (r == 0L) 0 else l - r)
                for (old in 0..<(1 shl m)) {
                    val newMask = old or mask
                    newdp[newMask] = min(newdp[newMask], (dp[old] + cost))
                }
            }
            dp = newdp
        }
        return dp[fullMask].toInt()
    }

    private fun gcd(a: Long, b: Long): Long {
        return if (b == 0L) a else gcd(b, a % b)
    }

    private fun lcm(a: Long, b: Long): Long {
        return a / gcd(a, b) * b
    }
}
```