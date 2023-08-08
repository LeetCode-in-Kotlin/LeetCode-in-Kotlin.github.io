[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2741\. Special Permutations

Medium

You are given a **0-indexed** integer array `nums` containing `n` **distinct** positive integers. A permutation of `nums` is called special if:

*   For all indexes `0 <= i < n - 1`, either `nums[i] % nums[i+1] == 0` or `nums[i+1] % nums[i] == 0`.

Return _the total number of special permutations. _As the answer could be large, return it **modulo **<code>10<sup>9 </sup>+ 7</code>.

**Example 1:**

**Input:** nums = [2,3,6]

**Output:** 2

**Explanation:** [3,6,2] and [2,6,3] are the two special permutations of nums.

**Example 2:**

**Input:** nums = [1,4,3]

**Output:** 2

**Explanation:** [3,1,4] and [4,1,3] are the two special permutations of nums.

**Constraints:**

*   `2 <= nums.length <= 14`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private var dp = HashMap<Pair<Int, Int>, Long>()
    private var adj = HashMap<Int, HashSet<Int>>()
    private var mod = 1000000007

    private fun count(destIdx: Int, set: Int): Long {
        if (Integer.bitCount(set) == 1) return 1
        val p = destIdx to set
        if (dp.containsKey(p)) {
            return dp[p]!!
        }
        var sum = 0L
        val newSet = set xor (1 shl destIdx)
        for (i in adj[destIdx]!!) {
            if ((set and (1 shl i)) == 0) continue
            sum += count(i, newSet) % mod
            sum %= mod
        }
        dp[p] = sum
        return sum
    }

    fun specialPerm(nums: IntArray): Int {
        for (i in nums.indices) adj[i] = hashSetOf()
        for ((i, vI) in nums.withIndex()) {
            for ((j, vJ) in nums.withIndex()) {
                if (vI != vJ && vI % vJ == 0) {
                    adj[i]!!.add(j)
                    adj[j]!!.add(i)
                }
            }
        }
        if (adj.all { it.value.size == nums.size - 1 }) {
            return (fact(nums.size.toLong()) % mod).toInt()
        }
        var total = 0
        for (i in nums.indices) {
            total += (count(i, (1 shl nums.size) - 1) % mod).toInt()
            total %= mod
        }
        return total
    }

    private fun fact(n: Long): Long {
        if (n == 1L) return n
        return n * fact(n - 1)
    }
}
```