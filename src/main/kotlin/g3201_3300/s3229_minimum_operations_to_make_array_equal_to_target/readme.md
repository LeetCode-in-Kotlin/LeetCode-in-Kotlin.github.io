[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3229\. Minimum Operations to Make Array Equal to Target

Hard

You are given two positive integer arrays `nums` and `target`, of the same length.

In a single operation, you can select any subarray of `nums` and increment or decrement each element within that subarray by 1.

Return the **minimum** number of operations required to make `nums` equal to the array `target`.

**Example 1:**

**Input:** nums = [3,5,1,2], target = [4,6,2,4]

**Output:** 2

**Explanation:**

We will perform the following operations to make `nums` equal to `target`:   
 \- Increment `nums[0..3]` by 1, `nums = [4,6,2,3]`.   
 \- Increment `nums[3..3]` by 1, `nums = [4,6,2,4]`.

**Example 2:**

**Input:** nums = [1,3,2], target = [2,1,4]

**Output:** 5

**Explanation:**

We will perform the following operations to make `nums` equal to `target`:   
 \- Increment `nums[0..0]` by 1, `nums = [2,3,2]`.   
 \- Decrement `nums[1..1]` by 1, `nums = [2,2,2]`.   
 \- Decrement `nums[1..1]` by 1, `nums = [2,1,2]`.   
 \- Increment `nums[2..2]` by 1, `nums = [2,1,3]`.   
 \- Increment `nums[2..2]` by 1, `nums = [2,1,4]`.

**Constraints:**

*   <code>1 <= nums.length == target.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], target[i] <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumOperations(nums: IntArray, target: IntArray): Long {
        val n = nums.size
        var incr: Long = 0
        var decr: Long = 0
        var ops: Long = 0
        for (i in 0 until n) {
            val diff = target[i] - nums[i]
            if (diff > 0) {
                if (incr < diff) {
                    ops += diff - incr
                }
                incr = diff.toLong()
                decr = 0
            } else if (diff < 0) {
                if (decr < -diff) {
                    ops += -diff - decr
                }
                decr = -diff.toLong()
                incr = 0
            } else {
                decr = 0
                incr = decr
            }
        }
        return ops
    }
}
```