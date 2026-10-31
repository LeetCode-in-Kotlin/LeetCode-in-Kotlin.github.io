[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3755\. Find Maximum Balanced XOR Subarray Length

Medium

Given an integer array `nums`, return the **length** of the **longest non-empty subarrays** that has a bitwise XOR of zero and contains an **equal** number of **even** and **odd** numbers. If no such subarray exists, return 0.

**Example 1:**

**Input:** nums = [3,1,3,2,0]

**Output:** 4

**Explanation:**

The subarray `[1, 3, 2, 0]` has bitwise XOR `1 XOR 3 XOR 2 XOR 0 = 0` and contains 2 even and 2 odd numbers.

**Example 2:**

**Input:** nums = [3,2,8,5,4,14,9,15]

**Output:** 8

**Explanation:**

The whole array has bitwise XOR `0` and contains 4 even and 4 odd numbers.

**Example 3:**

**Input:** nums = [0]

**Output:** 0

**Explanation:**

No non-empty subarray satisfies both conditions.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxBalancedSubarray(nums: IntArray): Int {
        val n = nums.size
        var ans = 0
        var xor = 0
        var diff = n
        val pos: MutableMap<Long?, Int?> = HashMap<Long?, Int?>(n + 1, 1f)
        pos[xor.toLong() shl 20 or diff.toLong()] = -1
        for (i in 0..<n) {
            xor = xor xor nums[i]
            diff += if (nums[i] % 2 != 0) 1 else -1
            val key = xor.toLong() shl 20 or diff.toLong()
            val j = pos[key]
            if (j != null) {
                ans = max(ans, i - j)
            } else {
                pos[key] = i
            }
        }
        return ans
    }
}
```