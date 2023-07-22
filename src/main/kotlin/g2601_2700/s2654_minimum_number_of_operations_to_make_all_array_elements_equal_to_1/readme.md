[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2654\. Minimum Number of Operations to Make All Array Elements Equal to 1

Medium

You are given a **0-indexed** array `nums` consisiting of **positive** integers. You can do the following operation on the array **any** number of times:

*   Select an index `i` such that `0 <= i < n - 1` and replace either of `nums[i]` or `nums[i+1]` with their gcd value.

Return _the **minimum** number of operations to make all elements of_ `nums` _equal to_ `1`. If it is impossible, return `-1`.

The gcd of two integers is the greatest common divisor of the two integers.

**Example 1:**

**Input:** nums = [2,6,3,4]

**Output:** 4

**Explanation:** We can do the following operations: 
- Choose index i = 2 and replace nums[2] with gcd(3,4) = 1. Now we have nums = [2,6,1,4]. 
- Choose index i = 1 and replace nums[1] with gcd(6,1) = 1. Now we have nums = [2,1,1,4]. 
- Choose index i = 0 and replace nums[0] with gcd(2,1) = 1. Now we have nums = [1,1,1,4]. 
- Choose index i = 2 and replace nums[3] with gcd(1,4) = 1. Now we have nums = [1,1,1,1].

**Example 2:**

**Input:** nums = [2,10,6,14]

**Output:** -1

**Explanation:** It can be shown that it is impossible to make all the elements equal to 1.

**Constraints:**

*   `2 <= nums.length <= 50`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

**Follow-up:**

The `O(n)` time complexity solution works, but could you find an `O(1)` constant time complexity solution?

## Solution

```kotlin
class Solution {
    fun minOperations(nums: IntArray): Int {
        var g = nums[0]
        var list = mutableListOf<Int>()
        var padding = 0
        var result = nums.size
        for (i in 0 until nums.size) {
            val n = nums[i]
            if (n == 1) {
                result--
            }
            g = gcd(g, n)
            if (i == nums.size - 1) continue
            val m = nums[i + 1]
            list.add(gcd(m, n))
        }
        if (g > 1) return -1
        while (!list.any { it == 1 }) {
            padding++
            val nlist = mutableListOf<Int>()
            for (i in 0 until list.size - 1) {
                val n = list[i]
                val m = list[i + 1]
                nlist.add(gcd(m, n))
            }
            list = nlist
        }
        return result + padding
    }

    private fun gcd(a: Int, b: Int): Int = if (b != 0) gcd(b, a % b) else a
}
```