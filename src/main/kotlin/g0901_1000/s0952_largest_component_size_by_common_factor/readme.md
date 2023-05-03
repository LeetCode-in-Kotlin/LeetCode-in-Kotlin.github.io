[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 952\. Largest Component Size by Common Factor

Hard

You are given an integer array of unique positive integers `nums`. Consider the following graph:

*   There are `nums.length` nodes, labeled `nums[0]` to `nums[nums.length - 1]`,
*   There is an undirected edge between `nums[i]` and `nums[j]` if `nums[i]` and `nums[j]` share a common factor greater than `1`.

Return _the size of the largest connected component in the graph_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex1.png)

**Input:** nums = [4,6,15,35]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex2.png)

**Input:** nums = [20,50,9,63]

**Output:** 2

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

**Input:** nums = [2,3,6,7,4,12,21,39]

**Output:** 8

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   All the values of `nums` are **unique**.

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    fun largestComponentSize(nums: IntArray): Int {
        var max = 0
        for (a in nums) {
            max = Math.max(max, a)
        }
        val roots = IntArray(max + 1)
        val sizes = IntArray(max + 1)
        for (idx in 1..max) {
            roots[idx] = idx
        }
        for (a in nums) {
            if (a == 1) {
                sizes[a] = 1
                continue
            }
            val sqrt = sqrt(a.toDouble()).toInt()
            val thisRoot = getRoot(roots, a)
            sizes[thisRoot]++
            for (factor in 1..sqrt) {
                if (a % factor == 0) {
                    val otherFactor = a / factor
                    val otherFactorRoot = getRoot(roots, otherFactor)
                    if (factor != 1) {
                        union(roots, thisRoot, factor, sizes)
                    }
                    union(roots, thisRoot, otherFactorRoot, sizes)
                }
            }
        }
        var maxConnection = 0
        for (size in sizes) {
            maxConnection = Math.max(maxConnection, size)
        }
        return maxConnection
    }

    private fun union(roots: IntArray, a: Int, b: Int, sizes: IntArray) {
        val rootA = getRoot(roots, a)
        val rootB = getRoot(roots, b)
        if (rootA != rootB) {
            sizes[rootA] += sizes[rootB]
            roots[rootB] = rootA
        }
    }

    private fun getRoot(roots: IntArray, a: Int): Int {
        if (roots[a] == a) {
            return a
        }
        roots[a] = getRoot(roots, roots[a])
        return roots[a]
    }
}
```