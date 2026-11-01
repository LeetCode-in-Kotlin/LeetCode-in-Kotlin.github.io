[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3939\. Count Non Adjacent Subsets in a Rooted Tree

Hard

You are given a rooted tree with `n` nodes labeled from 0 to `n - 1`, represented by an integer array `parent` of length `n`, where:

*   `parent[0] = -1` (node 0 is the root).
*   For each `1 <= i < n`, `parent[i]` is the parent of node `i` (`0 <= parent[i] < i`).

You are also given an integer array nums of length `n`, where `nums[i]` is the value of node `i`, and an integer `k`.

A non-empty subset of nodes is called **valid** if:

*   The **sum** of the values of the selected nodes is **divisible** by `k`.
*   No **two** selected nodes are **adjacent** in the tree (no node and its direct parent are both included in the subset).

Return the number of valid subsets modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** parent = [-1,0,1], nums = [1,2,3], k = 3

**Output:** 1

**Explanation:**

**![](https://assets.leetcode.com/uploads/2025/07/17/image1.png)**

The only valid subset is `{2}`. It contains node 2 with value 3, which is divisible by 3.

**Example 2:**

**Input:** parent = [-1,0,0,0], nums = [2,1,2,1], k = 3

**Output:** 2

**Explanation:**

**![](https://assets.leetcode.com/uploads/2025/07/17/image2.png)**

The valid subsets are:

*   `{1, 2}`: Nodes 1 and 2 are both children of node 0 and not directly connected to each other. Their values sum to `1 + 2 = 3`, which is divisible by 3.
*   `{2, 3}`: Nodes 2 and 3 are also non-adjacent. Their values sum to `2 + 1 = 3`, which is divisible by 3.

No other subset satisfies both conditions. Therefore, the answer is 2.

**Constraints:**

*   `n == parent.length == nums.length`
*   `1 <= n <= 1000`
*   `parent[0] == -1`
*   For all `1 <= i < n`:
    *   `0 <= parent[i] < i`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= 100`
*   `parent` describes a valid rooted tree.

## Solution

```kotlin
class Solution {
    fun countValidSubsets(parent: IntArray, nums: IntArray, k: Int): Int {
        val n = parent.size
        val h = IntArray(n)
        val to = IntArray(n - 1)
        val nx = IntArray(n - 1)
        buildTree(parent, h, to, nx)
        val dp = Array<Array<LongArray>>(n) { Array<LongArray>(2) { LongArray(k) } }
        dfs(0, h, to, nx, nums, k, dp)
        return getResult(dp)
    }

    private fun buildTree(parent: IntArray, h: IntArray, to: IntArray, nx: IntArray) {
        h.fill(-1)
        var idx = 0
        for (i in 1..<parent.size) {
            to[idx] = i
            nx[idx] = h[parent[i]]
            h[parent[i]] = idx++
        }
    }

    private fun dfs(
        u: Int,
        h: IntArray,
        to: IntArray,
        nx: IntArray,
        nums: IntArray,
        k: Int,
        dp: Array<Array<LongArray>>,
    ) {
        initializeDp(u, nums, k, dp)
        var e = h[u]
        while (e != -1) {
            val v = to[e]
            dfs(v, h, to, nx, nums, k, dp)
            mergeChild(dp[u], dp[v], k)
            e = nx[e]
        }
    }

    private fun initializeDp(u: Int, nums: IntArray, k: Int, dp: Array<Array<LongArray>>) {
        dp[u][0][0] = 1
        dp[u][1][nums[u] % k] = 1
    }

    private fun mergeChild(current: Array<LongArray>, child: Array<LongArray>, k: Int) {
        val next = Array<LongArray>(2) { LongArray(k) }
        mergeExcludedState(current[0], child, next[0], k)
        mergeSelectedState(current[1], child[0], next[1], k)
        copyState(next, current, k)
    }

    private fun mergeExcludedState(current: LongArray, child: Array<LongArray>, next: LongArray, k: Int) {
        for (a in 0..<k) {
            if (current[a] != 0L) {
                mergeWithChildStates(current[a], child[0], child[1], next, a, k)
            }
        }
    }

    private fun mergeSelectedState(current: LongArray, childExcluded: LongArray, next: LongArray, k: Int) {
        for (a in 0..<k) {
            if (current[a] != 0L) {
                mergeWithSingleChildState(current[a], childExcluded, next, a, k)
            }
        }
    }

    private fun mergeWithChildStates(
        currentWays: Long,
        childExcluded: LongArray,
        childSelected: LongArray,
        next: LongArray,
        currentMod: Int,
        k: Int,
    ) {
        for (childMod in 0..<k) {
            val childWays: Long = (childExcluded[childMod] + childSelected[childMod]) % MOD
            addWays(next, currentMod, childMod, currentWays, childWays, k)
        }
    }

    private fun mergeWithSingleChildState(
        currentWays: Long,
        child: LongArray,
        next: LongArray,
        currentMod: Int,
        k: Int,
    ) {
        for (childMod in 0..<k) {
            addWays(next, currentMod, childMod, currentWays, child[childMod], k)
        }
    }

    private fun addWays(target: LongArray, currentMod: Int, childMod: Int, currentWays: Long, childWays: Long, k: Int) {
        if (childWays == 0L) {
            return
        }
        val newMod = (currentMod + childMod) % k
        target[newMod] = (target[newMod] + currentWays * childWays) % MOD
    }

    private fun copyState(source: Array<LongArray>, target: Array<LongArray>, k: Int) {
        for (state in 0..1) {
            if (k >= 0) {
                System.arraycopy(source[state], 0, target[state], 0, k)
            }
        }
    }

    private fun getResult(dp: Array<Array<LongArray>>): Int {
        var result = dp[0][0][0] + dp[0][1][0]
        result %= MOD.toLong()
        result = (result - 1 + MOD) % MOD
        return result.toInt()
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```