[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3139\. Minimum Cost to Equalize Array

Hard

You are given an integer array `nums` and two integers `cost1` and `cost2`. You are allowed to perform **either** of the following operations **any** number of times:

*   Choose an index `i` from `nums` and **increase** `nums[i]` by `1` for a cost of `cost1`.
*   Choose two **different** indices `i`, `j`, from `nums` and **increase** `nums[i]` and `nums[j]` by `1` for a cost of `cost2`.

Return the **minimum** **cost** required to make all elements in the array **equal**_._

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [4,1], cost1 = 5, cost2 = 2

**Output:** 15

**Explanation:**

The following operations can be performed to make the values equal:

*   Increase `nums[1]` by 1 for a cost of 5. `nums` becomes `[4,2]`.
*   Increase `nums[1]` by 1 for a cost of 5. `nums` becomes `[4,3]`.
*   Increase `nums[1]` by 1 for a cost of 5. `nums` becomes `[4,4]`.

The total cost is 15.

**Example 2:**

**Input:** nums = [2,3,3,3,5], cost1 = 2, cost2 = 1

**Output:** 6

**Explanation:**

The following operations can be performed to make the values equal:

*   Increase `nums[0]` and `nums[1]` by 1 for a cost of 1. `nums` becomes `[3,4,3,3,5]`.
*   Increase `nums[0]` and `nums[2]` by 1 for a cost of 1. `nums` becomes `[4,4,4,3,5]`.
*   Increase `nums[0]` and `nums[3]` by 1 for a cost of 1. `nums` becomes `[5,4,4,4,5]`.
*   Increase `nums[1]` and `nums[2]` by 1 for a cost of 1. `nums` becomes `[5,5,5,4,5]`.
*   Increase `nums[3]` by 1 for a cost of 2. `nums` becomes `[5,5,5,5,5]`.

The total cost is 6.

**Example 3:**

**Input:** nums = [3,5,3], cost1 = 1, cost2 = 3

**Output:** 4

**Explanation:**

The following operations can be performed to make the values equal:

*   Increase `nums[0]` by 1 for a cost of 1. `nums` becomes `[4,5,3]`.
*   Increase `nums[0]` by 1 for a cost of 1. `nums` becomes `[5,5,3]`.
*   Increase `nums[2]` by 1 for a cost of 1. `nums` becomes `[5,5,4]`.
*   Increase `nums[2]` by 1 for a cost of 1. `nums` becomes `[5,5,5]`.

The total cost is 4.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>
*   <code>1 <= cost1 <= 10<sup>6</sup></code>
*   <code>1 <= cost2 <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minCostToEqualizeArray(nums: IntArray, cost1: Int, cost2: Int): Int {
        var max = 0L
        var min = Long.MAX_VALUE
        var sum = 0L
        for (num in nums) {
            if (num > max) {
                max = num.toLong()
            }
            if (num < min) {
                min = num.toLong()
            }
            sum += num
        }
        val n = nums.size
        var total = max * n - sum
        // When operation one is always better:
        if ((cost1 shl 1) <= cost2 || n <= 2) {
            return (total * cost1 % LMOD).toInt()
        }
        // When operation two is moderately better:
        var op1 = max(0L, (((max - min) shl 1L.toInt()) - total))
        var op2 = total - op1
        var result = (op1 + (op2 and 1L)) * cost1 + (op2 shr 1L.toInt()) * cost2
        // When operation two is significantly better:
        total += op1 / (n - 2L) * n
        op1 %= n - 2L
        op2 = total - op1
        result = min(result, ((op1 + (op2 and 1L)) * cost1 + (op2 shr 1L.toInt()) * cost2))
        // When operation two is always better:
        for (i in 0..1) {
            total += n.toLong()
            result = min(result, ((total and 1L) * cost1 + (total shr 1L.toInt()) * cost2))
        }
        return (result % LMOD).toInt()
    }

    companion object {
        private const val MOD = 1000000007
        private const val LMOD = MOD.toLong()
    }
}
```