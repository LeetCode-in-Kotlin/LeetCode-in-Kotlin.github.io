[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1703\. Minimum Adjacent Swaps for K Consecutive Ones

Hard

You are given an integer array, `nums`, and an integer `k`. `nums` comprises of only `0`'s and `1`'s. In one move, you can choose two **adjacent** indices and swap their values.

Return _the **minimum** number of moves required so that_ `nums` _has_ `k` _**consecutive**_ `1`_'s_.

**Example 1:**

**Input:** nums = [1,0,0,1,0,1], k = 2

**Output:** 1

**Explanation:** In 1 move, nums could be [1,0,0,0,1,1] and have 2 consecutive 1's.

**Example 2:**

**Input:** nums = [1,0,0,0,0,0,1,1], k = 3

**Output:** 5

**Explanation:** In 5 moves, the leftmost 1 can be shifted right until nums = [0,0,0,0,0,1,1,1].

**Example 3:**

**Input:** nums = [1,1,0,1], k = 2

**Output:** 0

**Explanation:** nums already has 2 consecutive 1's.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is `0` or `1`.
*   `1 <= k <= sum(nums)`

## Solution

```kotlin
class Solution {
    fun minMoves(nums: IntArray, k: Int): Int {
        val len = nums.size
        var cnt = 0
        var min = Long.MAX_VALUE
        for (num in nums) {
            if (num == 1) {
                cnt++
            }
        }
        val arr = IntArray(cnt)
        var idx = 0
        val sum = LongArray(cnt + 1)
        for (i in 0 until len) {
            if (nums[i] == 1) {
                arr[idx++] = i
                sum[idx] = sum[idx - 1] + i
            }
        }
        var i = 0
        while (i + k - 1 < cnt) {
            min = Math.min(min, getSum(arr, i, i + k - 1, sum))
            i++
        }
        return min.toInt()
    }

    private fun getSum(arr: IntArray, l: Int, h: Int, sum: LongArray): Long {
        val mid = l + (h - l) / 2
        val k = h - l + 1
        val radius = mid - l
        var res = sum[h + 1] - sum[mid + 1] - (sum[mid] - sum[l]) - (1 + radius) * radius
        if (k % 2 == 0) {
            res = res - arr[mid] - (radius + 1)
        }
        return res
    }
}
```