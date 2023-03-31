[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 823\. Binary Trees With Factors

Medium

Given an array of unique integers, `arr`, where each integer `arr[i]` is strictly greater than `1`.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return _the number of binary trees we can make_. The answer may be too large so return the answer **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [2,4]

**Output:** 3

**Explanation:** We can make these trees: `[2], [4], [4, 2, 2]`

**Example 2:**

**Input:** arr = [2,4,5,10]

**Output:** 7

**Explanation:** We can make these trees: `[2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2]`.

**Constraints:**

*   `1 <= arr.length <= 1000`
*   <code>2 <= arr[i] <= 10<sup>9</sup></code>
*   All the values of `arr` are **unique**.

## Solution

```kotlin
class Solution {
    private val dp: MutableMap<Int, Long> = HashMap()
    private val nums: MutableMap<Int, Int> = HashMap()
    fun numFactoredBinaryTrees(arr: IntArray): Int {
        arr.sort()
        for (i in arr.indices) {
            nums[arr[i]] = i
        }
        var ans: Long = 0
        for (i in arr.indices.reversed()) {
            ans = (ans % MOD + recursion(arr, arr[i], i) % MOD) % MOD
        }
        return ans.toInt()
    }

    private fun recursion(arr: IntArray, v: Int, idx: Int): Long {
        if (dp.containsKey(v)) {
            return dp[v]!!
        }
        var ret: Long = 1
        for (i in 0 until idx) {
            val child = arr[i]
            if (v % child == 0 && nums.containsKey(v / child)) {
                ret += (
                    (
                        recursion(arr, child, nums[arr[i]]!!) %
                            MOD
                            * recursion(arr, v / child, nums[v / child]!!) %
                            MOD
                        ) %
                        MOD
                    )
            }
        }
        dp[v] = ret
        return ret
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```