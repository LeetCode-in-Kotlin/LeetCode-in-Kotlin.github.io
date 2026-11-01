[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3877\. Minimum Removals to Achieve Target XOR

Medium

You are given an integer array `nums` and an integer `target`.

You may remove **any** number of elements from `nums` (possibly zero).

Return the **minimum** number of removals required so that the **bitwise XOR** of the remaining elements equals `target`. If it is impossible to achieve `target`, return -1.

The bitwise XOR of an empty array is 0.

**Example 1:**

**Input:** nums = [1,2,3], target = 2

**Output:** 1

**Explanation:**

*   Removing `nums[1] = 2` leaves `[nums[0], nums[2]] = [1, 3]`.
*   The XOR of `[1, 3]` is 2, which equals `target`.
*   It is not possible to achieve XOR = 2 in less than one removal, therefore the answer is 1.

**Example 2:**

**Input:** nums = [2,4], target = 1

**Output:** \-1

**Explanation:**

It is impossible to remove elements to achieve `target`. Thus, the answer is -1.

**Example 3:**

**Input:** nums = [7], target = 7

**Output:** 0

**Explanation:**

The XOR of all elements is `nums[0] = 7`, which equals `target`. Thus, no removal is needed.

**Constraints:**

*   `1 <= nums.length <= 40`
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>
*   <code>0 <= target <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minRemovals(nums: IntArray, target: Int): Int {
        var max = 0
        for (n in nums) {
            max = max(max, n)
        }
        val u = 1 shl (32 - Integer.numberOfLeadingZeros(max))
        if (target >= u) {
            return -1
        }
        val n = nums.size
        val f = Array<IntArray>(n + 1) { IntArray(u) }
        f[0].fill(Int.MAX_VALUE / 2)
        f[0][0] = 0
        for (i in 0..<n) {
            for (x in 0..<u) {
                f[i + 1][x] = min(f[i][x] + 1, f[i][x xor nums[i]])
            }
        }
        return if (f[n][target] == Int.MAX_VALUE / 2) -1 else f[n][target]
    }
}
```