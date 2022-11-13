[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 315\. Count of Smaller Numbers After Self

Hard

You are given an integer array `nums` and you have to return a new `counts` array. The `counts` array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example 1:**

**Input:** nums = [5,2,6,1]

**Output:** [2,1,1,0]

**Explanation:** To the right of 5 there are **2** smaller elements (2 and 1). To the right of 2 there is only **1** smaller element (1). To the right of 6 there is **1** smaller element (1). To the right of 1 there is **0** smaller element.

**Example 2:**

**Input:** nums = [-1]

**Output:** [0]

**Example 3:**

**Input:** nums = [-1,-1]

**Output:** [0,0]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import java.util.LinkedList

@Suppress("NAME_SHADOWING")
class Solution {
    fun countSmaller(nums: IntArray): List<Int> {
        var minVal = 10001
        var maxVal = -10001
        for (a in nums) {
            minVal = Math.min(minVal, a)
            maxVal = Math.max(maxVal, a)
        }
        val range = maxVal - (minVal - 1) + 1
        val offset = -(minVal - 1)
        val bit = FenwickTree(range)
        val ans = LinkedList<Int>()
        val n = nums.size
        var i = n - 1
        while (i >= 0) {
            bit.update(offset + nums[i], 1)
            ans.addFirst(bit.ps(offset + nums[i] - 1))
            i--
        }
        return ans
    }

    private class FenwickTree(n: Int) {
        // binary index tree, index 0 is not used
        var bit: IntArray
        var n: Int

        init {
            this.n = n + 1
            bit = IntArray(this.n)
        }

        fun update(i: Int, v: Int) {
            var i = i
            while (i < n) {
                bit[i] += v
                i += Integer.lowestOneBit(i)
            }
        }

        // prefix sum query
        fun ps(j: Int): Int {
            var j = j
            var ps = 0
            while (j != 0) {
                ps += bit[j]
                j -= Integer.lowestOneBit(j)
            }
            return ps
        }
    }
}
```