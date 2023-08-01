[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2709\. Greatest Common Divisor Traversal

Hard

You are given a **0-indexed** integer array `nums`, and you are allowed to **traverse** between its indices. You can traverse between index `i` and index `j`, `i != j`, if and only if `gcd(nums[i], nums[j]) > 1`, where `gcd` is the **greatest common divisor**.

Your task is to determine if for **every pair** of indices `i` and `j` in nums, where `i < j`, there exists a **sequence of traversals** that can take us from `i` to `j`.

Return `true` _if it is possible to traverse between all such pairs of indices,_ _or_ `false` _otherwise._

**Example 1:**

**Input:** nums = [2,3,6]

**Output:** true

**Explanation:** In this example, there are 3 possible pairs of indices: (0, 1), (0, 2), and (1, 2). To go from index 0 to index 1, we can use the sequence of traversals 0 -> 2 -> 1, where we move from index 0 to index 2 because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1, and then move from index 2 to index 1 because gcd(nums[2], nums[1]) = gcd(6, 3) = 3 > 1. To go from index 0 to index 2, we can just go directly because gcd(nums[0], nums[2]) = gcd(2, 6) = 2 > 1. Likewise, to go from index 1 to index 2, we can just go directly because gcd(nums[1], nums[2]) = gcd(3, 6) = 3 > 1.

**Example 2:**

**Input:** nums = [3,9,5]

**Output:** false

**Explanation:** No sequence of traversals can take us from index 0 to index 2 in this example. So, we return false.

**Example 3:**

**Input:** nums = [4,3,12,8]

**Output:** true

**Explanation:** There are 6 possible pairs of indices to traverse between: (0, 1), (0, 2), (0, 3), (1, 2), (1, 3), and (2, 3). A valid sequence of traversals exists for each pair, so we return true.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var map: MutableMap<Int, Int>? = null

    private lateinit var set: IntArray
    private fun findParent(u: Int): Int {
        return if (u == set[u]) u else findParent(set[u]).also { set[u] = it }
    }

    private fun union(a: Int, b: Int) {
        val p1 = findParent(a)
        val p2 = findParent(b)
        if (p1 != p2) {
            set[b] = p1
        }
        set[p2] = p1
    }

    private fun solve(n: Int, index: Int) {
        var n = n
        if (n % 2 == 0) {
            val x = map!!.getOrDefault(2, -1)
            if (x != -1) {
                union(x, index)
            }
            while (n % 2 == 0) n /= 2
            map!!.put(2, index)
        }
        val sqrt = kotlin.math.sqrt(n.toDouble()).toInt()
        for (i in 3..sqrt) {
            if (n % i == 0) {
                val x = map!!.getOrDefault(i, -1)
                if (x != -1) {
                    union(x, index)
                }
                while (n % i == 0) n /= i
                map!!.put(i, index)
            }
        }
        if (n > 2) {
            val x = map!!.getOrDefault(n, -1)
            if (x != -1) {
                union(x, index)
            }
            map!!.put(n, index)
        }
    }

    fun canTraverseAllPairs(nums: IntArray): Boolean {
        set = IntArray(nums.size)
        map = HashMap()
        for (i in nums.indices) set[i] = i
        for (i in nums.indices) solve(nums[i], i)
        val p = findParent(0)
        for (i in nums.indices) if (p != findParent(i)) return false
        return true
    }
}
```