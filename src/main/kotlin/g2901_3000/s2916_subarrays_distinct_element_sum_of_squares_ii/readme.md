[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2916\. Subarrays Distinct Element Sum of Squares II

Hard

You are given a **0-indexed** integer array `nums`.

The **distinct count** of a subarray of `nums` is defined as:

*   Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the **squares** of **distinct counts** of all subarrays of_ `nums`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** 15

**Explanation:** Six possible subarrays are: 

[1]: 1 distinct value 

[2]: 1 distinct value 

[1]: 1 distinct value 

[1,2]: 2 distinct values

[2,1]: 2 distinct values

[1,2,1]: 2 distinct values

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.

**Example 2:**

**Input:** nums = [2,2]

**Output:** 3

**Explanation:** Three possible subarrays are: 

[2]: 1 distinct value 

[2]: 1 distinct value

[2,2]: 1 distinct value 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var n = 0
    private lateinit var tree1: LongArray
    private lateinit var tree2: LongArray

    fun sumCounts(nums: IntArray): Int {
        n = nums.size
        tree1 = LongArray(n + 1)
        tree2 = LongArray(n + 1)
        var max = 0
        for (x in nums) {
            if (x > max) {
                max = x
            }
        }
        val last = IntArray(max + 1)
        var ans: Long = 0
        var cur: Long = 0
        for (i in 1..n) {
            val x = nums[i - 1]
            val j = last[x]
            cur += 2 * (query(i) - query(j)) + (i - j)
            ans += cur
            update(j + 1, 1)
            update(i + 1, -1)
            last[x] = i
        }
        return (ans % MOD).toInt()
    }

    private fun lowbit(index: Int): Int {
        return index and (-index)
    }

    private fun update(index: Int, x: Int) {
        var index = index
        val v = index * x
        while (index <= n) {
            tree1[index] += x.toLong()
            tree2[index] += v.toLong()
            index += lowbit(index)
        }
    }

    private fun query(index: Int): Long {
        var index = index
        var res: Long = 0
        val p = index + 1
        while (index > 0) {
            res += p * tree1[index] - tree2[index]
            index -= lowbit(index)
        }
        return res
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```