[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2926\. Maximum Balanced Subsequence Sum

Hard

You are given a **0-indexed** integer array `nums`.

A **subsequence** of `nums` having length `k` and consisting of **indices** <code>i<sub>0</sub> < i<sub>1</sub> < ... < i<sub>k-1</sub></code> is **balanced** if the following holds:

*   <code>nums[i<sub>j</sub>] - nums[i<sub>j-1</sub>] >= i<sub>j</sub> - i<sub>j-1</sub></code>, for every `j` in the range `[1, k - 1]`.

A **subsequence** of `nums` having length `1` is considered balanced.

Return _an integer denoting the **maximum** possible **sum of elements** in a **balanced** subsequence of_ `nums`.

A **subsequence** of an array is a new **non-empty** array that is formed from the original array by deleting some (**possibly none**) of the elements without disturbing the relative positions of the remaining elements.

**Example 1:**

**Input:** nums = [3,3,5,6]

**Output:** 14

**Explanation:** In this example, the subsequence [3,5,6] consisting of indices 0, 2, and 3 can be selected. 

nums[2] - nums[0] >= 2 - 0. 

nums[3] - nums[2] >= 3 - 2. 

Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums. 

The subsequence consisting of indices 1, 2, and 3 is also valid. 

It can be shown that it is not possible to get a balanced subsequence with a sum greater than 14.

**Example 2:**

**Input:** nums = [5,-1,-3,8]

**Output:** 13

**Explanation:** In this example, the subsequence [5,8] consisting of indices 0 and 3 can be selected. 

nums[3] - nums[0] >= 3 - 0. 

Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums. 

It can be shown that it is not possible to get a balanced subsequence with a sum greater than 13.

**Example 3:**

**Input:** nums = [-2,-1]

**Output:** -1

**Explanation:** In this example, the subsequence [-1] can be selected. It is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

@Suppress("NAME_SHADOWING")
class Solution {
    fun maxBalancedSubsequenceSum(nums: IntArray): Long {
        val n = nums.size
        var m = 0
        val arr = IntArray(n)
        var max = Int.MIN_VALUE
        for (i in 0 until n) {
            val x = nums[i]
            if (x > 0) {
                arr[m++] = x - i
            } else if (x > max) {
                max = x
            }
        }
        if (m == 0) {
            return max.toLong()
        }
        arr.sort()
        val map: MutableMap<Int, Int> = HashMap(m shl 1)
        var pre = Int.MIN_VALUE
        var index = 1
        for (x in arr) {
            if (x == pre) {
                continue
            }
            map[x] = index++
            pre = x
        }

        val bit = BIT(index)
        var ans: Long = 0
        for (i in 0 until n) {
            if (nums[i] <= 0) {
                continue
            }
            index = map[nums[i] - i]!!
            val cur = bit.query(index) + nums[i]
            bit.update(index, cur)
            ans = max(ans, cur)
        }
        return ans
    }

    private class BIT(var n: Int) {
        var tree: LongArray = LongArray(n + 1)

        fun lowbit(index: Int): Int {
            return index and (-index)
        }

        fun update(index: Int, v: Long) {
            var index = index
            while (index <= n && tree[index] < v) {
                tree[index] = v
                index += lowbit(index)
            }
        }

        fun query(index: Int): Long {
            var index = index
            var result: Long = 0
            while (index > 0) {
                result = max(tree[index].toDouble(), result.toDouble()).toLong()
                index -= lowbit(index)
            }
            return result
        }
    }
}
```