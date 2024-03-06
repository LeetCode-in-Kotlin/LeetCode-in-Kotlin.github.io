[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3040\. Maximum Number of Operations With the Same Score II

Medium

Given an array of integers called `nums`, you can perform **any** of the following operation while `nums` contains **at least** `2` elements:

*   Choose the first two elements of `nums` and delete them.
*   Choose the last two elements of `nums` and delete them.
*   Choose the first and the last elements of `nums` and delete them.

The **score** of the operation is the sum of the deleted elements.

Your task is to find the **maximum** number of operations that can be performed, such that **all operations have the same score**.

Return _the **maximum** number of operations possible that satisfy the condition mentioned above_.

**Example 1:**

**Input:** nums = [3,2,1,2,3,4]

**Output:** 3

**Explanation:** We perform the following operations: 
- Delete the first two elements, with score 3 + 2 = 5, nums = [1,2,3,4]. 
- Delete the first and the last elements, with score 1 + 4 = 5, nums = [2,3].
- Delete the first and the last elements, with score 2 + 3 = 5, nums = []. 

We are unable to perform any more operations as nums is empty.

**Example 2:**

**Input:** nums = [3,2,6,1,4]

**Output:** 2

**Explanation:** We perform the following operations: 
- Delete the first two elements, with score 3 + 2 = 5, nums = [6,1,4]. 
- Delete the last two elements, with score 1 + 4 = 5, nums = [6]. 

It can be proven that we can perform at most 2 operations.

**Constraints:**

*   `2 <= nums.length <= 2000`
*   `1 <= nums[i] <= 1000`

## Solution

```kotlin
import java.util.Objects
import kotlin.math.max

class Solution {
    private lateinit var nums: IntArray

    private var maxOps = 1

    private val dp: MutableMap<Pos, Int> = HashMap()

    private class Pos(var start: Int, var end: Int, var sum: Int) {
        override fun equals(other: Any?): Boolean {
            if (other == null) {
                return false
            }
            if (other !is Pos) {
                return false
            }
            return start == other.start && end == other.end && sum == other.sum
        }

        override fun hashCode(): Int {
            return Objects.hash(start, end, sum)
        }
    }

    fun maxOperations(nums: IntArray): Int {
        this.nums = nums
        val length = nums.size

        maxOperations(2, length - 1, nums[0] + nums[1], 1)
        maxOperations(0, length - 3, nums[length - 2] + nums[length - 1], 1)
        maxOperations(1, length - 2, nums[0] + nums[length - 1], 1)

        return maxOps
    }

    private fun maxOperations(start: Int, end: Int, sum: Int, nOps: Int) {
        if (start >= end) {
            return
        }

        if ((((end - start) / 2) + nOps) < maxOps) {
            return
        }

        val pos = Pos(start, end, sum)
        val posNops = dp[pos]
        if (posNops != null && posNops >= nOps) {
            return
        }
        dp[pos] = nOps
        if (nums[start] + nums[start + 1] == sum) {
            maxOps = max(maxOps, (nOps + 1))
            maxOperations(start + 2, end, sum, nOps + 1)
        }
        if (nums[end - 1] + nums[end] == sum) {
            maxOps = max(maxOps, (nOps + 1))
            maxOperations(start, end - 2, sum, nOps + 1)
        }
        if (nums[start] + nums[end] == sum) {
            maxOps = max(maxOps, (nOps + 1))
            maxOperations(start + 1, end - 1, sum, nOps + 1)
        }
    }
}
```