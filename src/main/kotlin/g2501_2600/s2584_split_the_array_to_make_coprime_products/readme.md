[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2584\. Split the Array to Make Coprime Products

Hard

You are given a **0-indexed** integer array `nums` of length `n`.

A **split** at an index `i` where `0 <= i <= n - 2` is called **valid** if the product of the first `i + 1` elements and the product of the remaining elements are coprime.

*   For example, if `nums = [2, 3, 3]`, then a split at the index `i = 0` is valid because `2` and `9` are coprime, while a split at the index `i = 1` is not valid because `6` and `3` are not coprime. A split at the index `i = 2` is not valid because `i == n - 1`.

Return _the smallest index_ `i` _at which the array can be split validly or_ `-1` _if there is no such split_.

Two values `val1` and `val2` are coprime if `gcd(val1, val2) == 1` where `gcd(val1, val2)` is the greatest common divisor of `val1` and `val2`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)

**Input:** nums = [4,7,8,15,3,5]

**Output:** 2

**Explanation:** The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i. The only valid split is at index 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)

**Input:** nums = [4,7,15,8,3,5]

**Output:** -1

**Explanation:** The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i. There is no valid split.

**Constraints:**

*   `n == nums.length`
*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    private fun fillDivideArray() {
        for (i in 1 until divideTo.size) {
            divideTo[i] = i
        }
        var i = 2
        while (i * i < divideTo.size) {
            if (divideTo[i] != i) {
                i++
                continue
            }
            var j = i + i
            while (j < divideTo.size) {
                if (divideTo[j] == j) {
                    divideTo[j] = i
                }
                j += i
            }
            i++
        }
    }

    fun findValidSplit(nums: IntArray): Int {
        if (divideTo[1] == 0) {
            fillDivideArray()
        }

        val dMap: MutableMap<Int, Int> = HashMap()
        val dividers: Array<MutableList<Int>?> = arrayOfNulls(nums.size)
        for (i in nums.indices) {
            dividers[i] = ArrayList()
            while (nums[i] != 1) {
                dividers[i]?.add(divideTo[nums[i]])
                dMap.put(divideTo[nums[i]], i)
                nums[i] = nums[i] / divideTo[nums[i]]
            }
        }
        var nextEnd = 0
        var i = 0
        while (i <= nextEnd) {
            for (j in dividers[i]!!.indices) {
                nextEnd = nextEnd.coerceAtLeast(dMap[dividers[i]!![j]]!!)
            }
            i++
        }
        return if (i == nums.size) -1 else i - 1
    }

    companion object {
        var divideTo = IntArray(1e6.toInt() + 1)
    }
}
```